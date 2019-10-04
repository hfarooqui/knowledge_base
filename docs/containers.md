# Docker

### ADD vs Copy

#### ADD
- If is a local tar archive is a recognized compression format (identity, gzip, bzip2 or xz) then it is unpacked as a directory. Resources from remote URLs are not decompressed.
- The best use for ADD is local tar file auto-extraction into the image, as in ADD rootfs.tar.xz /
- Because image size matters, using ADD to fetch packages from remote URLs is strongly discouraged; you should use curl or wget instead. That way you can delete the files you no longer need after they’ve been extracted and you don’t have to add another layer in your image. For example, you should avoid doing things like:
```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
And instead, do something like:
```
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```
#### COPY
- Same as 'ADD', but without the tar and remote URL handling.
- COPY is preferred. That’s because it’s more transparent than ADD
- COPY only supports the basic copying of local files into the container. COPY <src> <dest>
- If you are copying multiple files to same destination directory in Dockerfile, COPY them individually, rather than all at once. This ensures that each step’s build cache is only invalidated (forcing the step to be re-run) if the specifically required files change.
  E.g.
  ```
  COPY requirements.txt /tmp/
  RUN pip install --requirement /tmp/requirements.txt
  COPY . /tmp/
  ```
