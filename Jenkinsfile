/*
 * Copyright (c) 2019-present Sonatype, Inc. All rights reserved.
 * Includes the third-party code listed at http://links.sonatype.com/products/nexus/attributions.
 * "Sonatype" is a trademark of Sonatype, Inc.
 */
@Library(['private-pipeline-library', 'jenkins-shared', 'int-jenkins-shared']) _

dockerizedBuildPipeline(
  prepare: {
    githubStatusUpdate('pending')
  },
  buildAndTest: {
    sh './build.sh'
  },
  skipVulnerabilityScan: true,
  archiveArtifacts: 'docs/*',
  testResults: [],
  onSuccess: {
    buildNotifications(currentBuild, env, 'master')
  },
  onFailure: {
    buildNotifications(currentBuild, env, 'master')
  }
)
