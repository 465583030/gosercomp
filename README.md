## Golang ���л������л�������ܱȽ�


### ���Ե� Serializers
��golang�Դ���*encoding/json*��*encoding/xml*Ϊ��׼�������������ܱȽϺõļ������л��⡣
* [encoding/json](http://golang.org/pkg/encoding/json/)
* [encoding/xml](http://golang.org/pkg/encoding/xml/)
* [github.com/youtube/vitess/go/bson](http://github.com/youtube/vitess/go/bson)
* [github.com/philhofer/msgp](http://github.com/philhofer/msgp)
* [github.com/golang/protobuf](http://github.com/golang/protobuf)
* [github.com/gogo/protobuf](http://github.com/gogo/protobuf)
* [github.com/google/flatbuffers](http://github.com/google/flatbuffers)


### �ų��� Serializers
���� alecthomas ���е�[����](https://github.com/alecthomas/go_serialization_benchmarks)������Ŀ��������ܵ�ԭ��û�н��в��ԡ�

* [encoding/gob](http://golang.org/pkg/encoding/gob/)
* [github.com/alecthomas/binary](http://github.com/alecthomas/binary)
* [github.com/davecgh/go-xdr/xdr](http://github.com/davecgh/go-xdr/xdr)
* [github.com/ugorji/go/codec](http://github.com/ugorji/go/codec)
* [labix.org/v2/mgo/bson](http://labix.org/v2/mgo/bson)
* [github.com/DeDiS/protobuf](http://github.com/DeDiS/protobuf)
* [gopkg.in/vmihailenco/msgpack.v2](http://gopkg.in/vmihailenco/msgpack.v2)

### ���Ի���
����`github.com/youtube/vitess/go/bson`���������Ҫ��װ `goimports`��`codegen`:
``` go
go get github.com/youtube/vitess/go/bson
go get golang.org/x/tools/cmd/goimports
go get github.com/youtube/vitess/tree/master/go/cmd/bsongen
bsongen -file data.go -o bson_data.go -type ColorGroup
```
���� `MessagePack`������Ҫ��װ���Լ�����`go generate`������ص���:
``` go
go get github.com/tinylib/msgp
go generate
```

����`ProtoBuf`,����Ҫ��װ[protoc������](https://github.com/google/protobuf/releases)���Լ�protoc���Լ�������ص��ࣺ
``` go
go get github.com/golang/protobuf
go generate
```

����`gogo/protobuf`,����Ҫ��װ���Լ�������ص��ࣺ
``` go
go get github.com/gogo/protobuf/gogoproto
go get github.com/gogo/protobuf/protoc-gen-gofast
go generate
```

����`flatbuffers`,����Ҫ��װ[flatbuffers������](https://github.com/google/flatbuffers/releases), �Լ�flatbuffers�⣺
``` go
github.com/google/flatbuffers/go
go generate
```

��ʵ�ϣ�����ͨ��`go generate`������ص��࣬��Ҳ����ͨ�����������ɣ���ο�`data.go`�е�ע�͡�


����������������:
```
go test -bench=.
```
### ��������
���еĲ��Ի������µ�struct,�Զ����ɵ�struct�� ����protobufҲ�ʹ˽ṹ����һ�¡�
``` go
type ColorGroup struct {
	ID     int `json:"id" xml:"id,attr""`
	Name   string `json:"name" xml:"name"`
	Colors []string `json:"colors" xml:"colors"`
}
```

    
### ���ܲ��Խ��
<pre>
BenchmarkMarshalByJson-4                      1000000              1877 ns/op
BenchmarkUnmarshalByJson-4                  300000                4099 ns/op

BenchmarkMarshalByXml-4                       200000                8315 ns/op
BenchmarkUnmarshalByXml-4                   100000                26627 ns/op

BenchmarkMarshalByBson-4                      500000                3518 ns/op
BenchmarkUnmarshalByBson-4                  1000000              1778 ns/op

BenchmarkMarshalByMsgp-4                     5000000              292 ns/op
BenchmarkUnmarshalByMsgp-4                 3000000              543 ns/op

BenchmarkMarshalByProtoBuf-4                1000000              1011 ns/op
BenchmarkUnmarshalByProtoBuf-4            1000000              1750 ns/op

BenchmarkMarshalByGogoProtoBuf-4        5000000              220 ns/op
BenchmarkUnmarshalByGogoProtoBuf-4    2000000              901 ns/op

BenchmarkMarshalByFlatBuffers-4              3000000              566 ns/op
BenchmarkUnmarshalByFlatBuffers-4          50000000            9.54 ns/op
BenchmarUmByFlatBuffers_withFields-4      3000000              554 ns/op
</pre>

��β��Խ����ࡣ
�ӽ������������ **MessagePack**,**gogo/protobuf**,��**flatbuffers**��࣬����������Ŀ������л��ͷ����л��ϸ���ǧ����Ҷ��ǿ����Եġ�
�ӱ������������������ѡ��**MessagePack**��**gogo/protobuf**�����ԣ����߶��д����á�
flatbuffers�е㷴���࣬��Ϊ���Ĳ����ܵײ㣬���Ҵӽ�������������л�������Ҫ��һ�㡣��������һ���ô����Ǿ��������ֻ��Ҫ�ض����ֶΣ�
�����뽫���е��ֶζ������л����ӽ���Ͽ����������л��ֶ�ÿ������ֻ����9.54���룬������Ϊ�ֶ�ֻ���ڱ����ʵ�ʱ��Ŵ�byte����ת��Ϊ��Ӧ�����͡�
���������ĳ����£����������N�������ܡ��������л��Ĵ��������̫�ѿ��ˡ�



