install otelcollector.exe

netstat -ano | findstr :4317 -> cari port
taskkill /PID 6452 /F -> kill pid (administrator)
java -javaagent:opentelemetry-javaagent.jar "-Dotel.exporter.otlp.endpoint=http://0.0.0.0:4317" "-Dotel.service.name=java-test" "-Dotel.exporter.otlp.protocol=grpc" -jar build/libs/dice-0.0.1-SNAPSHOT.jar -> running with otel collector
java -javaagent:opentelemetry-javaagent.jar "-Dotel.exporter.otlp.endpoint=http://localhost:8200" "-Dotel.service.name=java-test2" -jar build/libs/dice-0.0.1-SNAPSHOT.jar -> running without otel collector, through apm server
java -javaagent:opentelemetry-javaagent.jar "-Dotel.exporter.otlp.endpoint=http://0.0.0.0:4317" "-Dotel.service.name=java-test" "-Dotel.exporter.otlp.protocol=grpc" -jar build/libs/dice-0.0.1-SNAPSHOT.jar --server.port=8081 -> custom port

pull image -> docker pull otel/opentelemetry-collector:latest
running docker first time -> docker run -d -p 4317:4317 -p 4318:4318 --name otelcol-local -v /e/Metrodata/BNI/otelcollector/collector-config.yaml:/etc/otel/config.yaml otel/opentelemetry-collector:latest --config=/etc/otel/config.yaml
stop -> docker stop <name_container or id>
running again -> docker run -d -p 4317:4317 -p 4318:4318 -v /e/Metrodata/BNI/otelcollector/collector-config.yaml:/etc/otel/config.yaml otel/opentelemetry-collector:latest --config=/etc/otel/config.yaml

details:
-d -> detached mode, akan berjalan pada background
-p 4317:4317 -p 4318:4318 -> port mappings
--name otelcol-local -> memberi nama pada container
-v /e/Metrodata/BNI/otelcollector/collector-config.yaml:/etc/otel/config.yaml -> volume mount, mount file/folder dari host ke container
otel/opentelemetry-collector:latest -> nama dari image yg digunakan
--config=/etc/otel/config.yaml -> argumen otelcol untuk menggunakan konfigurasi pada file yang ada pada path tersebut, path yg di-mount

