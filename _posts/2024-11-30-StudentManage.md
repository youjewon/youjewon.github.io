---

layout : single
title : "학생관리 시스템 구현"
date: 2024-11-30
categories :
 - 
tags : 
  - C++
---


# [CPP] 학생관리 시스템 구현

### 클래스로 학생관리시스템을 구현하며 학습
기능 :
* 학생의 이름, 학번, 성정으로 클래스 설계
* 학생 추가, 삭제, 조회 기능 구현
* 성정 평균, 계산

학습 포인트 :
* 클래스와 객체의 기본 사용
* 데이터 캡슐화 및 접근자/설정자 (getter/setter) 사용
* 동적 배열(std::vector) 활용

확장: 추후 추가
* 파일 입출력으로 데이터를 저장/불러오기
* 상속을 활용해 학부생, 대원상 클래스 구분 

## 기본 설계
Student 클래스 : 
* 맴버 변수 :
    * std::string name : 학생 이름
    * int id : 학번
    * int degree : 성적 / 현재 int 성적을 학점 배열로 변경 예정
* 맴버 함수 :
    * 생성자 : Student(std::string name, int id)
    * 접근자/설정자(getter/setter) : getName(), getId(), getdegree();

Management 클래스 :
* 맴버 변수 :
    * std::vector<Student> students : 학생목록
* 맴버 함수 : 
    * void addStudent(const Student& student) : 학생 추가
    * void removeStudent(int id) : 학생 제거
    * Student* findStudent(int id) : 학생 조회
    * void displayAllStudents() : 전체 학생 정보 출력

## 전체 코드

```cpp
/*
    학생 관리 시스템 구현하기
    11/30
    1. 학생 객채를 만들고 학생을 추가, 삭제, 조회 하는 코드 작성
*/

#include<iostream>
#include<vector>
#include<string>

// 학생 클래스 이름, 학번, 학점을 갖고 각 정보를 캡슐화하여 getter로 반환
class Student {
private :
    std::string name;
    int id;
    int degree;         // 평균 학점을 시작으로 수정하기 
public :
    Student (std::string name,int id) : name(name), id(id) {}
    
    std::string getName() const{ return name; }
    int getId() const { return id; }
    int getdegree() const { return degree; }
};

// 관리클래스를 통해 학생을 추가, 제거, 조회 가능
class Management {
private :
    std::vector<Student> students;

public :
    // 학생 추가
    void addStudent(const Student& student) {
        students.push_back(student);
    }
    // 학생 제거
    void removeStudent(int id) {
        for(auto it = students.begin(); it != students.end(); ++it) {
            if(it->getId() == id) {
                students.erase(it);
                std::cout << "student ereased!!" << id << std::endl;
                return;
            }
        }
        std::cout << "Id : " << id << "is Not founded" << std::endl;
    }
    // 학생 조회
    Student* findStudent(int id) {
        for(auto& student : students) {
            if(student.getId() == id) {
                return &student;
            }
        }
        return nullptr;
    }
    // 모든 학생 출력
    void displayAllStudnets() const {
        std::cout << "All Students : \n";
        for(const auto& student : students) {
            std::cout << "ID " << student.getId() << ", Name " << student.getName()
                      << ", degree" << student.getdegree() << std::endl;
        }
    }
};


int main() {
    Management manager;

    // 예제 데이터 추가
    manager.addStudent(Student("Alice", 1));
    manager.addStudent(Student("Bob", 2));

    // 학생 검색 및 성적 추가
    Student* student = manager.findStudent(1);
    
    // 학생 삭제
    manager.removeStudent(2);

    // 전체 학생 출력
    manager.displayAllStudnets();

    return 0;
}
```
