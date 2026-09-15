---
title: "CF 104687A - \u0422\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a"
description: "Chúng ta có ba số nguyên, mỗi số biểu thị độ dài cạnh tiềm năng của một tam giác. Nhiệm vụ là xác định xem ba độ dài này có thể tạo thành một tam giác hợp lệ hay không."
date: "2026-06-29T08:45:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "A"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 57
verified: true
draft: false
---

[CF 104687A - \u0422\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a](https://codeforces.com/problemset/problem/104687/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba số nguyên, mỗi số biểu thị độ dài cạnh tiềm năng của một tam giác. Nhiệm vụ là xác định xem ba độ dài này có thể tạo thành một tam giác hợp lệ hay không. 

Về mặt hình học, ba đoạn tạo thành một tam giác chỉ khi không có đoạn nào quá dài để hai đoạn còn lại có thể “đóng” lại với nhau. Theo thuật ngữ đại số, điều này chuyển thành điều kiện là tổng của hai cạnh bất kỳ phải lớn hơn cạnh thứ ba. Vì chúng ta đang xử lý ba giá trị nên chúng ta cần kiểm tra điều kiện này cho cả ba hoán vị. 

Kích thước đầu vào là tối thiểu: chính xác ba số nguyên, mỗi số từ 1 đến 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ phương pháp nào thậm chí quét một số lượng nhỏ các điều kiện không đổi là đủ. Không cần cấu trúc dữ liệu, vòng lặp trên phạm vi lớn hoặc mối quan tâm tối ưu hóa. Kiểm tra thời gian liên tục là lớp phức tạp có ý nghĩa duy nhất ở đây. 

Trường hợp cạnh chính xuất phát từ sự bình đẳng ở ranh giới. Một độc giả ngây thơ có thể chấp nhận nhầm những trường hợp như`1 2 3`vì tổng bằng cạnh thứ ba nhưng đẳng thức không tạo thành tam giác. Một trường hợp khó phát hiện khác là khi các giá trị không được sắp xếp; Ví dụ,`3 1 2`vẫn nên bị từ chối mặc dù giá trị lớn nhất không ở vị trí cuối cùng. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là áp dụng trực tiếp định nghĩa. Chúng ta kiểm tra cả ba bất đẳng thức: liệu`a + b > c`,`a + c > b`, Và`b + c > a`. Nếu tất cả đều đúng thì các cạnh sẽ tạo thành một hình tam giác. 

Không có cách nào có ý nghĩa để đơn giản hóa điều này hơn nữa vì vấn đề đã có kích thước không đổi. Một cách tiếp cận “ngây thơ” vẫn có thể cố gắng hoán vị tất cả thứ tự của các cạnh và kiểm tra tính hợp lệ của tam giác trên mỗi hoán vị, nhưng điều đó dẫn đến sự lặp lại không cần thiết. Với ba giá trị, điều đó có nghĩa là kiểm tra sáu hoán vị, mỗi hoán vị yêu cầu tối đa ba phép so sánh, vẫn là thời gian không đổi nhưng dư thừa. 

Quan sát quan trọng là tính hợp lệ của tam giác chỉ phụ thuộc vào tổng từng cặp so với cạnh còn lại. Khi nhận ra tính đối xứng, chúng ta có thể kiểm tra trực tiếp cả ba điều kiện hoặc sắp xếp các cạnh và quy điều kiện thành một bất đẳng thức duy nhất: sau khi sắp xếp sao cho`x ≤ y ≤ z`, chúng ta chỉ cần xác minh`x + y > z`. Việc sắp xếp mang lại cấu trúc rõ ràng hơn và tránh suy luận về hoán vị. 

Cả hai cách tiếp cận đều hợp lệ, nhưng phiên bản đã sắp xếp thường được ưa thích hơn trong lập trình cạnh tranh vì nó tổng quát hóa rõ ràng cho các chiều cao hơn hoặc các vấn đề tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các bất đẳng thức) | O(1) | O(1) | Đã chấp nhận | 
| Sắp xếp + kiểm tra một lần | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng phương pháp sắp xếp để làm rõ. 

1. Đọc ba số nguyên đầu vào. 
2. Lưu trữ chúng trong một danh sách. 
3. Sắp xếp danh sách theo thứ tự không giảm. 
4. Đặt các giá trị được sắp xếp là`x`,`y`, Và`z`, Ở đâu`z`là cạnh lớn nhất. 
5. Kiểm tra xem`x + y > z`. 
6. Nếu điều kiện đúng, xuất ra`YES`, nếu không thì xuất ra`NO`. 

Bước sắp xếp không phải là về hiệu suất mà là về việc giảm độ phức tạp của lý luận. Khi phần tử lớn nhất bị cô lập, điều kiện tam giác sẽ chuyển thành một bất đẳng thức duy nhất thay vì ba. 

### Tại sao nó hoạt động 

Một tam giác chỉ không thể tồn tại khi một cạnh ít nhất bằng tổng của hai cạnh kia. Việc sắp xếp đảm bảo chúng ta luôn so sánh cạnh lớn nhất với tổng của hai cạnh còn lại. Nếu bất đẳng thức đó đúng thì hai bất đẳng thức còn lại tự động được thỏa mãn vì cả hai đều có tổng ít nhất bằng`x + y`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())
    sides = [a, b, c]
    sides.sort()

    if sides[0] + sides[1] > sides[2]:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Mã đọc ba giá trị trong một dòng, lưu trữ chúng trong danh sách và sắp xếp chúng sao cho giá trị lớn nhất nằm ở cuối. Điểm quyết định duy nhất là kiểm tra bất đẳng thức tam giác giữa hai giá trị nhỏ hơn và giá trị lớn nhất. Việc so sánh rất chặt chẽ, loại bỏ chính xác các trường hợp suy biến trong đó tổng bằng cạnh thứ ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1 1 1`| Bước | Bên | Các mặt được sắp xếp | Kiểm tra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | [1, 1, 1] | | | 
| Sắp xếp | [1, 1, 1] | [1, 1, 1] | | 
| Bất bình đẳng | | [1, 1, 1] | 1 + 1 > 1 | 

Bất đẳng thức xảy ra nên đầu ra là`YES`. Điều này tương ứng với một tam giác đều trong đó tất cả các cạnh đều bằng nhau và thỏa mãn điều kiện tam giác. 

### Ví dụ 2:`1 2 3`| Bước | Bên | Các mặt được sắp xếp | Kiểm tra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | [1, 2, 3] | | | 
| Sắp xếp | [1, 2, 3] | [1, 2, 3] | | 
| Bất bình đẳng | | [1, 2, 3] | 1 + 2 > 3 | 

Ở đây tổng bằng cạnh lớn nhất, không hẳn là lớn hơn, do đó điều kiện không thành công và kết quả đầu ra là`NO`. Điều này đại diện cho một tam giác suy biến thu gọn thành một đường thẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp ba phần tử là công việc liên tục và chỉ thực hiện một so sánh sau đó | 
| Không gian | O(1) | Chỉ một danh sách có kích thước cố định gồm ba số nguyên được lưu trữ | 

Các ràng buộc là cực kỳ nhỏ nên ngay cả việc triển khai trực tiếp nhất cũng diễn ra ngay lập tức trong giới hạn. Không có mối quan tâm về chi phí bộ nhớ hoặc hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, c = map(int, input().split())
    sides = [a, b, c]
    sides.sort()
    return "YES\n" if sides[0] + sides[1] > sides[2] else "NO\n"

# provided samples
assert run("1 1 1") == "YES\n"
assert run("1 2 3") == "NO\n"

# custom cases
assert run("2 3 4") == "YES\n", "valid small triangle"
assert run("1 1 2") == "NO\n", "degenerate equality case"
assert run("100 1 1") == "NO\n", "large imbalance case"
assert run("5 5 9") == "YES\n", "near boundary valid case"
assert run("10 10 20") == "NO\n", "strict equality boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 4 | CÓ | tam giác hợp lệ cơ bản | 
| 1 1 2 | KHÔNG | trường hợp cạnh bình đẳng | 
| 100 1 1 | KHÔNG | mất cân bằng cực độ | 
| 5 5 9 | CÓ | trường hợp hợp lệ gần biên | 
| 10 10 20 | KHÔNG | thực thi bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi cạnh lớn nhất bằng tổng của hai cạnh còn lại. Ví dụ, đầu vào`10 10 20`. 

Sau khi sắp xếp, chúng tôi nhận được`[10, 10, 20]`. Thuật toán kiểm tra`10 + 10 > 20`, đánh giá là`20 > 20`, SAI. Đầu ra là chính xác`NO`. Điều này xác nhận rằng sự bình đẳng được xử lý một cách chính xác và ngăn chặn các tam giác “phẳng” suy biến. 

Một trường hợp khác là khi đầu vào không được sắp xếp theo thứ tự, chẳng hạn như`3 1 2`. Sắp xếp biến đổi điều này thành`[1, 2, 3]`, và phép kiểm tra bất đẳng thức tương tự sẽ bác bỏ nó một cách chính xác. Điều này cho thấy độ chính xác không phụ thuộc vào thứ tự đầu vào mà chỉ phụ thuộc vào độ lớn tương đối.
