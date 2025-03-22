### Recommender Service Performance/Load Testing

# Tools
- Locust (https://locust.io/)
- Python3
- pip

# Scripts
Load testing script suite is available at https://github.siriusxm.com/TAG/sequencer2-locust-loadtest/tree/master/Atlas_LoadTest.  If you don't have access to this repo, request a zipped copy of the repo from the Recommender Services team.

# Testing Instructions
- Go to the directory where the test suite exists
- Launch locust on the command line
- Navigate in a browser to http://localhost:8089
  - set the host to the desired target deployment
  - configure the number of users, spawn rate and duration
