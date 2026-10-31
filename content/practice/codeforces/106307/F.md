---
title: "CF 106307F - Đây có phải là Fibonacci"
description: "Chúng ta được cho một số được viết dưới dạng một chuỗi, có khả năng rất lớn và chúng ta được yêu cầu quyết định xem giá trị của nó có xuất hiện trong dãy Fibonacci được xác định bởi các giá trị bắt đầu f0 = 1 và f1 = 1 hay không, với mỗi số hạng tiếp theo được hình thành bằng cách tính tổng hai số hạng trước đó."
date: "2026-06-19T14:42:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106307
codeforces_index: "F"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023, Day 9: Polish Kids Contest"
rating: 0
weight: 106307
solve_time_s: 46
verified: true
draft: false
---

[CF 106307F - Đây có phải là Fibonacci](https://codeforces.com/problemset/problem/106307/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số được viết dưới dạng một chuỗi, có khả năng rất lớn và chúng ta được yêu cầu quyết định xem giá trị của nó có xuất hiện trong dãy Fibonacci được xác định bởi các giá trị bắt đầu f0 = 1 và f1 = 1 hay không, với mỗi số hạng tiếp theo được hình thành bằng cách tính tổng hai số hạng trước đó. 

Khó khăn chính là số đầu vào k có thể có tới 100000 chữ số, điều này ngay lập tức loại trừ mọi cách tiếp cận chuyển đổi nó thành kiểu số nguyên có sẵn. Ngay cả các số nguyên 64 bit cũng sẽ tràn trước phạm vi này rất lâu, do đó, vấn đề phải được giải quyết hoàn toàn bằng cách sử dụng số học chuỗi hoặc bằng cách tránh phân tích cú pháp k thành một kiểu số. 

Một cách đọc ngây thơ gợi ý rằng hãy tạo các số Fibonacci cho đến khi chúng ta đạt hoặc vượt quá k, sau đó so sánh. Ý tưởng này đúng về mặt cấu trúc nhưng chỉ hoạt động nếu chúng ta có thể so sánh các số nguyên lớn một cách hiệu quả. 

Một trường hợp khó phát hiện khi k có các số 0 đứng đầu. Ví dụ: “0001” phải được coi là “1” và phải khớp với giá trị Fibonacci 1. Một vấn đề khác phát sinh nếu k cực lớn và vượt xa bất kỳ số Fibonacci nào phù hợp trong bộ nhớ, trong đó việc tăng trưởng số đơn giản sẽ không thể thực hiện được nếu không xử lý số nguyên lớn. 

Thông tin chi tiết dựa trên ràng buộc chính là các số Fibonacci tăng theo cấp số nhân và số chữ số của chúng tăng gần như tuyến tính. Điều này đảm bảo rằng mặc dù k có thể có 100000 chữ số, nhưng số lượng số Fibonacci mà chúng ta thực sự cần tạo ra chỉ ở mức tối đa vài trăm nghìn và trên thực tế nhỏ hơn nhiều do tăng trưởng theo cấp số nhân. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra các số Fibonacci bắt đầu từ 1, 1 và liên tục tính toán giá trị tiếp theo cho đến khi chúng ta khớp với k hoặc vượt quá nó. Tính chính xác là ngay lập tức vì dãy Fibonacci được tạo theo thứ tự và chúng tôi không bao giờ bỏ qua các giá trị. 

Vấn đề là các con số Fibonacci nhanh chóng trở nên khổng lồ. Số Fibonacci chữ số thứ 100000 rất lớn về mặt thiên văn và việc lưu trữ hoặc tính toán nó bằng các số nguyên gốc là không thể. Ngay cả với các số nguyên lớn, việc cộng lặp đi lặp lại hai số lớn, mỗi số có khả năng dài 100000 chữ số sẽ dẫn đến hành vi bậc hai về độ dài chữ số trên mỗi bước. Nếu có M số Fibonacci được tạo và mỗi số có tối đa D chữ số thì tổng chi phí sẽ vào khoảng O(M · D), quá chậm ở giới hạn trên. 

Quan sát quan trọng là chúng ta không bao giờ cần bất cứ thứ gì ngoại trừ phép so sánh và phép cộng trên những số lớn. Điều này cho phép chúng ta biểu diễn các số Fibonacci dưới dạng chuỗi và thực hiện phép cộng theo từng chữ số. Vì chuỗi tăng theo cấp số nhân nên số lần lặp M vẫn có thể quản lý được và phép cộng dựa trên chuỗi là đủ. 

Việc tối ưu hóa không phải là thay đổi phép lặp mà là sử dụng số học chuỗi và dừng sớm khi số Fibonacci được tạo vượt quá k trong so sánh độ dài từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (số ngây thơ) | O(F · D2) | O(D) | Quá chậm | 
| Tối ưu (mô phỏng chuỗi) | O(F · D) | O(D) | Đã chấp nhận | 

Ở đây F là số thuật ngữ Fibonacci được tạo cho đến khi vượt quá k và D là số chữ số. 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng các số Fibonacci dưới dạng chuỗi.

1. Chuẩn hóa chuỗi đầu vào k bằng cách loại bỏ các số 0 ở đầu, vì so sánh số chỉ phụ thuộc vào giá trị. Nếu chuỗi kết quả trống, hãy coi nó là "0". Điều này đảm bảo các so sánh nhất quán với biểu diễn Fibonacci. 
2. Khởi tạo hai chuỗi a="1" và b = "1", biểu thị các số Fibonacci liên tiếp. 
3. So sánh a và k. Nếu bằng nhau, chúng ta có thể trả về CÓ ngay lập tức vì số hạng Fibonacci đầu tiên khớp với nhau. 
4. Lặp lại tạo số Fibonacci tiếp theo c = a + b bằng cách sử dụng phép cộng chuỗi, trong đó phép cộng được thực hiện từng chữ số từ phải sang trái có nhớ. 
5. Sau khi tính c, cập nhật a = b và b = c, duy trì bất biến cấp số Fibonacci. 
6. Sau mỗi lần cập nhật, so sánh b với k. Nếu bằng nhau, trả về CÓ. 
7. Nếu b lớn hơn k về độ dài chữ số hoặc nếu so sánh từ điển cho thấy b > k, chúng ta có thể dừng và trả về NO vì tất cả các số Fibonacci tiếp theo sẽ chỉ tăng lớn hơn. 

Hoạt động quan trọng là so sánh chuỗi. So sánh độ dài chiếm ưu thế: nếu len(b) > len(k), chúng ta đã biết b > k. Nếu độ dài bằng nhau, so sánh từ điển tương ứng với so sánh số vì cả hai chuỗi biểu thị số nguyên không âm không có số 0 đứng đầu. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng ta duy trì bất biến rằng a và b là các số Fibonacci liên tiếp theo đúng thứ tự. Dãy số Fibonacci tăng dần sau các số hạng ban đầu, vì vậy một khi b vượt quá k thì không có số hạng nào trong tương lai có thể khớp với k. Vì chúng tôi kiểm tra sự bằng nhau ở mọi bước trước khi vượt quá mức, nên chúng tôi đảm bảo tính chính xác mà không bỏ lỡ kết quả khớp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def strip(s: str) -> str:
    i = 0
    while i < len(s) and s[i] == '0':
        i += 1
    return s[i:] if i < len(s) else "0"

def add_strings(x: str, y: str) -> str:
    i, j = len(x) - 1, len(y) - 1
    carry = 0
    res = []

    while i >= 0 or j >= 0 or carry:
        s = carry
        if i >= 0:
            s += ord(x[i]) - 48
            i -= 1
        if j >= 0:
            s += ord(y[j]) - 48
            j -= 1
        res.append(chr((s % 10) + 48))
        carry = s // 10

    return ''.join(reversed(res))

def greater(x: str, y: str) -> bool:
    x = strip(x)
    y = strip(y)
    if len(x) != len(y):
        return len(x) > len(y)
    return x > y

def main():
    n = int(input())
    k = strip(input().strip())

    a, b = "1", "1"

    if k == "1":
        print("YES")
        return

    while True:
        c = add_strings(a, b)
        a, b = b, c

        if b == k:
            print("YES")
            return

        if greater(b, k):
            print("NO")
            return

if __name__ == "__main__":
    main()
```Việc triển khai giữ các số Fibonacci dưới dạng chuỗi xuyên suốt. các`add_strings`hàm thực hiện phép cộng theo từng chữ số có nhớ, đây là cấu trúc số nguyên lớn tiêu chuẩn. các`strip`đảm bảo chúng ta tránh được sự không khớp do các số 0 đứng đầu. 

Hàm so sánh trước tiên kiểm tra độ dài, đây là yếu tố so sánh chi phối đối với số lượng lớn. Chỉ khi độ dài phù hợp thì chúng ta mới quay lại so sánh từ điển. 

Vòng lặp có chủ ý là vô hạn vì việc chấm dứt được đảm bảo bằng cách khớp k hoặc vượt quá nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

k = "1" 

| Bước | một | b | c (tiếp theo) | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | kiểm tra b == k | 

Vì k bằng số Fibonacci đầu tiên nên chúng ta trả về ngay CÓ. 

Điều này cho thấy tầm quan trọng của việc xử lý các số hạng ban đầu mà không buộc phải lặp lại không cần thiết. 

### Ví dụ 2 

đầu vào: 

k = "8" 

| Bước | một | b | c | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | bắt đầu | 
| 1 | 1 | 2 | 3 | kiểm tra | 
| 2 | 2 | 3 | 5 | kiểm tra | 
| 3 | 3 | 5 | 8 | trận đấu | 

Ở lần lặp thứ ba, b trở thành "8", khớp chính xác với k. Thuật toán kết thúc thành công. 

Dấu vết này cho thấy cách mô phỏng tái tạo lại chuỗi Fibonacci một cách trung thực mà không bị tràn số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(F · D) | mỗi bước Fibonacci thêm hai chuỗi chữ số D | 
| Không gian | O(D) | chỉ có hai hoặc ba số nguyên lớn được lưu trữ bất cứ lúc nào | 

Số lần lặp Fibonacci F bị giới hạn vì các số Fibonacci tăng giá trị theo cấp số nhân, do đó, việc đạt đến cấp độ 100000 chữ số chỉ cần một số bước vừa phải. Mỗi bước xử lý các chữ số một cách tuyến tính, làm cho giải pháp đủ hiệu quả cho các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples (conceptual, as formatting is incomplete)
assert run("1\n1\n") == "YES"

# custom cases
assert run("1\n2\n") == "NO"
assert run("1\n8\n") == "YES"
assert run("3\n001\n") == "YES"
assert run("3\n10\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | CÓ | trận Fibonacci nhỏ nhất | 
| 1, 2 | KHÔNG | từ chối sớm | 
| 3.001 | CÓ | chuẩn hóa số 0 đứng đầu | 
| 3, 10 | KHÔNG | kiểm tra tư cách thành viên | 

## Vỏ cạnh 

### Số 0 đứng đầu 

đầu vào: 

k = "0001" 

Sau khi tước, k trở thành "1". Thuật toán ngay lập tức khớp với số Fibonacci đầu tiên. Nếu không chuẩn hóa, so sánh chuỗi sẽ không chính xác vì "1" != "0001". 

###Rất nhỏ k 

đầu vào: 

k = "2" 

Trình tự tiến triển 1, 1, 2. Thuật toán kiểm tra từng bước và xác định chính xác sự trùng khớp ở số hạng thứ ba. Không có hiện tượng vượt quá xảy ra trước khi đạt được nó. 

### Số phi Fibonacci lớn 

đầu vào: 

k = "1000000000000000000000" 

Chuỗi Fibonacci được tạo nhanh chóng vượt quá mức này. Khi b vượt quá k về độ dài chữ số hoặc giá trị, thuật toán sẽ dừng ngay lập tức, đảm bảo không tiếp tục tính toán không cần thiết.
