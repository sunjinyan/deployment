# 启动编译环境
FROM golang:1.16-alpine AS builder



# 配置编译环境
RUN go env -w GO111MODULE=on

RUN go env -w GOPROXY=https://goproxy.cn,direct


# 拷贝源代码到镜像中
COPY  .  /go/src/coolcar/server

# 编译
WORKDIR /go/src/coolcar/server
RUN  go install ./gateway/...


#docker 多阶段编译  --from中的builder对应的就是顶部第一阶段中的builder
FROM alpine:3.15
COPY --from=builder /go/bin/gateway /bin/gateway

ENV ADDR=:8080

EXPOSE 8080

# 设置服务入口
ENTRYPOINT [ "/bin/gateway" ]

#CMD 与 ENTRYPOINT  不变的写在 ENTRYPOINT中，动态可变、可被用户输入所覆盖的参数写在CMD中
#CMD ["executable"]