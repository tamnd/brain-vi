---
title: "CF 1420A - Phân loại khối"
description: "Bài toán đưa ra cho chúng ta một dãy các hình khối, mỗi hình có thể tích nguyên dương. Các hình khối ban đầu được sắp xếp theo một thứ tự tùy ý."
date: "2026-06-11T06:37:37+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1420
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 672 (Div. 2)"
rating: 900
weight: 1420
solve_time_s: 70
verified: true
draft: false
---

[CF 1420A - Sắp xếp khối](https://codeforces.com/problemset/problem/1420/A) 

**Xếp hạng:** 900 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra cho chúng ta một dãy các hình khối, mỗi hình có thể tích nguyên dương. Các hình khối ban đầu được sắp xếp theo một thứ tự tùy ý. Mục tiêu của chúng ta là xác định xem liệu chúng ta có thể sắp xếp các khối theo thứ tự không giảm chỉ bằng cách hoán đổi các khối lân cận hay không, với ràng buộc là Wheatley sẽ từ bỏ nhiệm vụ nếu số lần hoán đổi cần đạt đến mức tối đa có thể đối với một chuỗi có kích thước đó, tức là$n \cdot (n-1)/2 - 1$. Về mặt khái niệm, điều này có nghĩa là Wheatley sẵn sàng thực hiện hầu hết mọi quy trình sắp xếp, ngoại trừ trường hợp cực kỳ hiếm khi chuỗi đảo ngược hoàn toàn với thứ tự sắp xếp và chỉ chứa hai phần tử. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số khối và thể tích của chúng. Đầu ra là "CÓ" đơn giản nếu các khối có thể được sắp xếp theo các ràng buộc thiếu kiên nhẫn của Wheatley và "KHÔNG" nếu ngược lại. 

Những ràng buộc cho chúng ta biết rằng$n$có thể đạt được$5 \cdot 10^4$cho một trường hợp thử nghiệm riêng lẻ, với tổng số tích lũy$10^5$trên tất cả các trường hợp thử nghiệm. Với giới hạn thời gian 1 giây, điều này sẽ loại bỏ mọi giải pháp có kết quả tệ hơn$O(n \log n)$cho mỗi trường hợp thử nghiệm cho toàn bộ đầu vào, mặc dù các giải pháp tuyến tính hoặc gần tuyến tính là lý tưởng. Bản thân hoạt động hoán đổi có thể được hình dung như các chuyển động sắp xếp bong bóng, nhưng việc mô phỏng trực tiếp mọi giao dịch hoán đổi sẽ quá chậm đối với hoạt động lớn nhất.$n$. 

Một trường hợp cạnh không rõ ràng xảy ra với chuỗi có đúng hai hình khối. Ví dụ, nếu trình tự là`[2, 1]`, trao đổi duy nhất có thể thực hiện được là một trao đổi duy nhất, nhưng Wheatley sẽ chỉ chấp nhận các chuỗi yêu cầu ít hơn$1$tráo đổi. Trường hợp cạnh này tạo ra "KHÔNG" mặc dù về nguyên tắc mảng có thể sắp xếp được. Các chuỗi trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như`[2, 2, 2]`, đã được sắp xếp và có thể chấp nhận được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mô phỏng sắp xếp bong bóng. Đối với mỗi cặp liền kề, chúng tôi hoán đổi chúng nếu chúng không đúng thứ tự và đếm số lần hoán đổi. Sau khi kết thúc, chúng tôi so sánh số lượng trao đổi với$n \cdot (n-1)/2 - 1$. Điều này đúng vì sắp xếp bong bóng chỉ sử dụng các hoán đổi lân cận và sẽ luôn sắp xếp mảng. Tuy nhiên, trong trường hợp xấu nhất, điều này đòi hỏi$O(n^2)$trao đổi, điều này không thực tế đối với$n = 5 \cdot 10^4$. 

Cái nhìn sâu sắc quan trọng là Wheatley chỉ từ chối sắp xếp các chuỗi có độ dài bằng hai và giảm dần. Đối với các chuỗi có độ dài lớn hơn hai, số lần hoán đổi cần thiết trong trường hợp xấu nhất có thể xảy ra hoàn toàn nhỏ hơn$n \cdot (n-1)/2 - 1$. Lý do là bất kỳ chuỗi nào gồm ba phần tử trở lên luôn có thể được sắp xếp bằng cách sử dụng các hoán đổi lân cận với thời gian ít hơn ngưỡng. Vì vậy, chúng ta không cần phải thực sự mô phỏng các giao dịch hoán đổi. Thay vào đó, chúng ta có thể kiểm tra hai điều kiện: nếu chuỗi có chính xác hai phần tử và phần tử đầu tiên lớn hơn phần tử thứ hai thì câu trả lời là "KHÔNG". Trong tất cả các trường hợp khác, câu trả lời là "CÓ". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng sắp xếp bong bóng Brute Force | O(n^2) | O(1) | Quá chậm đối với n lớn | 
| Kiểm tra dựa trên ngưỡng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,$t$. 
2. Với mỗi test, hãy đọc số lập phương,$n$, và danh sách các tập$a$. 
3. Nếu$n = 2$Và$a[0] > a[1]$, in "NO" và tiếp tục test case tiếp theo. Điều này xử lý tình huống duy nhất vượt quá ngưỡng trao đổi. 
4. Trong tất cả các trường hợp khác, hãy in "CÓ". Bất kỳ chuỗi nào có nhiều hơn hai phần tử hoặc chuỗi gồm hai phần tử theo thứ tự không giảm đều có thể được sắp xếp trong giới hạn hoán đổi. 

Tại sao nó hoạt động: Điều bất biến là bất kỳ chuỗi nào có độ dài lớn hơn hai luôn có thể được sắp xếp với ít hơn$n \cdot (n-1)/2 - 1$trao đổi lân cận. Ngoại lệ duy nhất là chuỗi hai phần tử theo thứ tự giảm dần nghiêm ngặt, yêu cầu chính xác một lần hoán đổi, vượt quá ngưỡng. Điều này đảm bảo tính chính xác mà không cần mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    if n == 2 and a[0] > a[1]:
        print("NO")
    else:
        print("YES")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Nó kiểm tra trường hợp cạnh duy nhất trong đó mảng hai phần tử theo thứ tự giảm dần nghiêm ngặt. Tất cả các chuỗi khác ngay lập tức tạo ra "CÓ", tránh tính toán không cần thiết. Việc sử dụng`map(int, input().split())`chuyển đổi chuỗi đầu vào thành số nguyên một cách chính xác, ngay cả đối với chuỗi lớn$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[5, 3, 2, 1, 4]`| Bước | n | một | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | [5, 3, 2, 1, 4] | n > 2 | CÓ | 

Tất cả các chuỗi dài hơn hai đều được chấp nhận, bất kể thứ tự ban đầu. Thuật toán trả về "CÓ". 

### Ví dụ 2 

đầu vào:`[2, 1]`| Bước | n | một | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [2, 1] | n = 2 và a[0] > a[1] | KHÔNG | 

Chuỗi duy nhất không đạt giới hạn hoán đổi là mảng giảm dần hai phần tử. Thuật toán trả về chính xác "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm yêu cầu đọc n số nguyên và một phép so sánh. | 
| Không gian | O(n) | Không gian để lưu trữ danh sách các tập cho từng trường hợp thử nghiệm. | 

Cho rằng tổng n trên tất cả các trường hợp thử nghiệm tối đa là 10^5, điều này nằm trong giới hạn thời gian. Việc sử dụng bộ nhớ được chấp nhận dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 2 and a[0] > a[1]:
            print("NO")
        else:
            print("YES")
    return out.getvalue().strip()

# provided samples
assert run("3\n5\n5 3 2 1 4\n6\n2 2 2 2 2 2\n2\n2 1\n") == "YES\nYES\nNO", "sample 1"

# custom cases
assert run("2\n2\n1 2\n3\n3 1 2\n") == "YES\nYES", "2-element ascending, 3-element unordered"
assert run("1\n2\n5 4\n") == "NO", "2-element descending"
assert run("1\n3\n1 1 1\n") == "YES", "all equal"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "descending of length >2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2, 3 3 1 2 | CÓ, CÓ | Xử lý đúng mảng 2 phần tử tăng dần và 3 phần tử không có thứ tự | 
| 2 5 4 | KHÔNG | Xác định chính xác mảng giảm dần 2 phần tử | 
| 3 1 1 1 | CÓ | Xử lý các phần tử bằng nhau | 
| 5 5 4 3 2 1 | CÓ | Mảng giảm dần dài hơn vẫn có thể sắp xếp trong giới hạn | 

## Vỏ cạnh 

Đối với mảng giảm dần hai phần tử`[2, 1]`, thuật toán kiểm tra`n == 2 and a[0] > a[1]`và trả về "KHÔNG". Vì`[1, 2]`, điều kiện không thành công và thuật toán trả về "CÓ". Vì`[5, 4, 3, 2, 1]`, độ dài vượt quá hai, do đó việc kiểm tra không thành công và được trả về "CÓ". Vì`[2, 2, 2]`, tất cả các phần tử đều bằng nhau, đáp ứng yêu cầu không giảm và được trả về "CÓ". Những trường hợp này xác nhận rằng điều kiện duy nhất nắm bắt được tất cả các lỗi không rõ ràng và giải pháp đó xử lý tất cả các tình huống khác một cách hiệu quả.
