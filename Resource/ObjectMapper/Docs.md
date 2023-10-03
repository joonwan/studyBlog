
`ObjectMapper`는 JSON을 읽고 쓰는 기능을 제공하며, 이를 기본 POJOs (Plain Old Java Objects) 또는 일반적인 목적의 JSON Tree Model (JsonNode)과 상호 변환할 수 있습니다. 또한 관련된 변환 작업을 수행하기 위한 기능도 제공하며, 다양한 JSON 콘텐츠 스타일 및 다른 객체 개념(다형성 및 객체 식별성과 같은)을 지원하도록 매우 사용자 정의할 수 있습니다. 또한 `ObjectMapper`는 고급 `ObjectReader` 및 `ObjectWriter` 클래스의 팩토리 역할을 하며, Mapper 및 이로부터 생성된 ObjectReaders 및 ObjectWriters는 JSON의 실제 읽기/쓰기를 구현하기 위해 JsonParser 및 JsonGenerator의 인스턴스를 사용합니다. 참고로 대부분의 읽기 및 쓰기 메서드가 이 클래스를 통해 노출되지만, 일부 기능은 ObjectReader 및 ObjectWriter를 통해서만 노출되며 특히 더 긴 값 시퀀스의 읽기/쓰기는 ObjectReader.readValues(InputStream) 및 ObjectWriter.writeValues(OutputStream)을 통해서만 사용할 수 있습니다.