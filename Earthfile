VERSION 0.8

FROM ghcr.io/sapphiccode/containers/alpine-cicd:latest

RUN git config --global user.name "automata" && \
    git config --global user.email "automata@sapphiccode.net"

WORKDIR /work
RUN git clone https://github.com/beetbox/beets.git .
RUN git remote add fork-artemist https://github.com/artemist/beets.git && git fetch fork-artemist

build:
    ARG --required tag
    ARG branch=$tag
    RUN git switch -c tmp $tag
    RUN git cherry-pick a53fe00fe6026caf223d905960891cda60251ce9
    RUN git rm -r .github && git commit -m 'ci: disable GitHub special files'
    RUN --push --secret remote \
        git remote add new-origin "$remote" && \
        git push --force new-origin tmp:$branch

main:
    LET latest=$(git tag --list 'v*' --sort=-version:refname | head -n 1)
    BUILD +build --tag=$latest --branch=main
    BUILD +build --tag=$latest
