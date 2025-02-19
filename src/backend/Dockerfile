FROM rust as build

# create a new empty shell project
RUN USER=root cargo new --bin lambda_backend
WORKDIR /lambda_backend

# copy over your manifests
COPY ./Cargo.toml ./Cargo.toml

# this build step will cache your dependencies
RUN cargo build --release
RUN rm src/*.rs

# copy your source tree
COPY ./src ./src

# build for release
RUN rm ./target/release/deps/lambda_backend*
RUN cargo build --release

# our final base
FROM rust

# copy the build artifact from the build stage
COPY --from=build /lambda_backend/target/release/lambda_backend .

# set the startup command to run your binary
CMD ["./lambda_backend"]