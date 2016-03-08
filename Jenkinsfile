// vim: ai ts=2 sts=2 et sw=2 ft=groovy et
echo "Hello from pipeline"
node ("aws-centos-7-SELinux-T2Medium"){
   git url:'https://github.com/ClusterHQ/ceph-flocker-driver.git', branch:'ceph-provision'
   dir ("flocker") {
     git url:'https://github.com/ClusterHQ/flocker.git', branch:'tp-test'
   }
   sh 'rm -rf v'
   sh 'virtualenv v'
   sh 'v/bin/pip install . ./flocker[dev]  python-subunit junitxml https://twistedmatrix.com/Releases/pre/16.0.0pre2/Twisted-16.0.0pre2.tar.bz2'
   sh 'touch ${HOME}/.ssh/known_hosts'
   sshagent(['9a0d57ba-6ade-4d01-85da-a1b910fd8dda']) {
     sh 'v/bin/python flocker/admin/run-acceptance-tests --distribution centos-7 --provider aws --config-file /tmp/acceptance.yaml --branch master --dataset-backend ceph_flocker_driver -- --reporter=subunit'
   }
   sh 'cat trial.log | subunit-1to2 | subunit2junitxml --no-passthrough --output-to=results.xml'
   archiveJunit '**/results.xml'
}
