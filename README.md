# api-response class

class ApiResponse<T extends Serializable> {
  bool? success;
  int? code;
  T? data;
  String? message;

  ApiResponse({this.success, this.data, this.message, this.code});

  factory ApiResponse.fromJson(Map<String, dynamic> json, Function(Map<String, dynamic>) create) {
    dynamic data;
    if(json['data'] != null){
      data = create(json['data']);
    } else {
      data = null;
    }

    return ApiResponse<T>(
      success: json["success"],
      code: json["code"],
      message: json["message"],
      data: data,
    );
  }
}

class ApiResponseList<T extends Serializable> {
  bool? success;
  int? code;
  List<T>? data;
  String? message;

  ApiResponseList({this.success, this.code, this.data, this.message});

  factory ApiResponseList.fromJson(Map<String, dynamic> json, T Function(Map<String, dynamic>) create) {
    List<dynamic> jsonDataList = json["data"];
    List<T> dataList = jsonDataList.map<T>((item) => create(item)).toList();

    return ApiResponseList<T>(
      success: json["success"],
      code: json["code"],
      data: dataList,
      message: json["message"],
    );
  }

  Map<String, dynamic> toJson() => {
    "success": this.success,
    "code": this.code,
    "data": this.data?.map((item) => item.toJson()).toList(),
    "message" : this.message
  };



}

abstract class Serializable {
  Map<String, dynamic> toJson();
}



#example class

class Test {
  test() {
    // For single object
    var json = {
      "success": true,
      "data": {
        "name": "",
        "phone": "",
      },
      "message": "First time entrance",
      "code": 200
    };
    var response = ApiResponse<User>.fromJson(json, (data) => User.fromJson(data));

    // For list object
    var json2 = {
      "success": true,
      "data": [
        {
          "name": "",
          "phone": "",
        },
        {
          "name": "",
          "phone": "",
        }
       ],
      "message": "First time entrance",
      "code": 200
    };
    var responseList = ApiResponseList<User>.fromJson(json2, (data) => User.fromJson(data));
  }
}

#model class

class User implements Serializable{
  int? id;
  String? name;
  String? phone;
  
  User({this.id, this.name, this.phone});

  User.fromJson(Map<String, dynamic> json) {
    id = json['id'];
    name = json['name'];
    phone = json['phone'];
  }

 @override
  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = Map<String, dynamic>();
    data['id'] = this.id;
    data['name'] = this.name;
    data['phone'] = this.phone;
    return data;
  }
}  
