FROM golang:1.17.13 as gofuzzbuilder
ARG project='decimal'
ARG fuzzTargetPath='fuzz/SetString'

ADD . ${project}
WORKDIR ${project}/${fuzzTargetPath}
RUN go get -u github.com/dvyukov/go-fuzz/go-fuzz@latest github.com/dvyukov/go-fuzz/go-fuzz-build@latest
RUN GO_ENABLED=0 go-fuzz-build -libfuzzer -o /setstring-fuzz.a .

FROM silkeh/clang:latest as sanitizer
COPY --from=gofuzzbuilder /setstring-fuzz.a setstring-fuzz.a
RUN clang -fsanitize=fuzzer /setstring-fuzz.a -o /setstring-fuzz.libfuzzer

FROM ubuntu:20.04
COPY --from=sanitizer /setstring-fuzz.libfuzzer /
