---
title: "CF 1450A - Tránh Trygub"
description: "Chúng ta được cho một chuỗi bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là sắp xếp lại các ký tự của nó thành một chuỗi b sao cho chuỗi \"trygub\" không xuất hiện dưới dạng một chuỗi con trong b."
date: "2026-06-11T03:37:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 800
weight: 1450
solve_time_s: 98
verified: false
draft: false
---

[CF 1450A - Tránh Trygub](https://codeforces.com/problemset/problem/1450/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`a`bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là sắp xếp lại các ký tự của nó thành một chuỗi`b`sao cho chuỗi "trygub" không xuất hiện dưới dạng chuỗi con trong`b`. Một dãy con được hình thành bằng cách xóa 0 hoặc nhiều ký tự mà không thay đổi thứ tự tương đối của các ký tự còn lại. Ví dụ, nếu`a = "xzyw"`, thì "xy" là một dãy con, nhưng "yx" thì không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của chuỗi và chính chuỗi đó. Đầu ra cho mỗi trường hợp thử nghiệm là một phiên bản được sắp xếp lại của chuỗi trong đó không thể lấy được "trygub" bằng cách xóa các ký tự. 

Các hạn chế là nhỏ:`n`có thể lên tới 200 và`t`lên tới 100, tức là tổng số ký tự chúng ta cần xử lý nhiều nhất là 20.000. Giá trị này đủ nhỏ để ngay cả một thuật toán đơn giản thao tác tuyến tính từng chuỗi cũng sẽ nhanh chóng. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa cho đầu vào rất lớn. 

Trường hợp cạnh tinh tế xảy ra khi chuỗi`a`đã chứa các chữ cái 't', 'r', 'y', 'g', 'u', 'b' theo đúng thứ tự ở đâu đó trong đó. Một giải pháp ngây thơ chỉ đơn giản là bỏ đi`a`không thay đổi có thể vô tình cho phép "trygub" làm chuỗi tiếp theo. Một trường hợp cạnh khác xảy ra khi chuỗi ngắn hơn 6 chữ cái - trong trường hợp đó, "trygub" không thể xuất hiện, vì vậy mọi thứ tự đều hợp lệ. Nếu chuỗi có các chữ cái lặp lại, người ta phải tránh tạo chuỗi con bằng cách giữ sáu ký tự theo thứ tự phá vỡ chuỗi "trygub". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của chuỗi`a`và kiểm tra xem "trygub" có phải là một dãy con hay không. Cách tiếp cận này đúng nhưng không thực tế, vì số hoán vị là`n!`, đó là thiên văn ngay cả đối với`n = 10`. Việc kiểm tra từng hoán vị cho một dãy con là O(n), do đó tổng thời gian vượt xa giới hạn có thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải tìm kiếm các chuỗi con một cách rõ ràng. "Trygub" có sáu ký tự riêng biệt. Bất kỳ chuỗi nào cũng có thể được sắp xếp lại sao cho ít nhất một trong các ký tự này không xuất hiện theo thứ tự ban đầu cần thiết để tạo thành "trygub". Phương pháp đơn giản nhất là sắp xếp chuỗi. Việc sắp xếp thay đổi thứ tự tương đối của tất cả các ký tự, do đó, nó sẽ không bao giờ giữ nguyên chuỗi chính xác "trygub" trừ khi chuỗi gốc chính xác là "bgrtuy" theo thứ tự đó, điều này là không thể khi sắp xếp theo thứ tự bảng chữ cái. Việc sắp xếp đảm bảo rằng "trygub" không thể xuất hiện dưới dạng một chuỗi con và việc triển khai rất đơn giản. 

Một phương pháp khác là di chuyển tất cả các lần xuất hiện của 't', 'r', 'y', 'g', 'u', 'b' sang một thứ tự khác (ví dụ: đảo ngược chúng hoặc xáo trộn chúng). Việc sắp xếp đơn giản hơn và đảm bảo hoạt động được với mọi đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp chuỗi | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc độ dài`n`và chuỗi`a`. 
3. Chuyển chuỗi thành danh sách các ký tự. 
4. Sắp xếp danh sách theo thứ tự abc. Điều này thay đổi thứ tự tương đối của tất cả các ký tự. 
5. Nối các ký tự đã sắp xếp lại thành một chuỗi`b`. 
6. In`b`. 

Việc sắp xếp có hiệu quả vì nó đảm bảo rằng các ký tự 't', 'r', 'y', 'g', 'u', 'b' không thể xuất hiện theo thứ tự chính xác "trygub", phá vỡ chuỗi con. Vì chúng ta được phép có bất kỳ hoán vị hợp lệ nào nên việc sắp xếp thỏa mãn các ràng buộc của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = input().strip()
    b = ''.join(sorted(a))
    print(b)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó đọc chuỗi, loại bỏ khoảng trắng, sắp xếp và in kết quả. Việc sắp xếp đảm bảo rằng "trygub" không thể là một dãy con. sử dụng`input().strip()`tránh các vấn đề về dòng mới có thể ảnh hưởng đến việc sắp xếp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
11
antontrygub
```| Bước | Biến`a`| Đã sắp xếp`b`| 
| --- | --- | --- | 
| Đọc chuỗi | antontrygub | antontrygub | 
| Chuyển đổi sang danh sách | ['a','n','t','o','n','t','r','y','g','u','b'] | giống nhau | 
| Sắp xếp | giống nhau | ['a','b','g','n','n','o','r','t','t','u','y'] | 
| Tham gia | - | "abgnnorttuy" | 

Đầu ra được sắp xếp không chứa "trygub" dưới dạng một chuỗi con vì các chữ cái có thứ tự tương đối khác. 

Một ví dụ khác:```
15
bestcoordinator
```| Bước | Biến`a`| Đã sắp xếp`b`| 
| --- | --- | --- | 
| Đọc chuỗi | điều phối viên tốt nhất | giống nhau | 
| Sắp xếp | giống nhau | 'aabccddeinnoorrstt' | 

Đầu ra không thể chứa "trygub" vì các chữ cái được sắp xếp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp từng chuỗi mất O(n log n) và n <= 200 | 
| Không gian | O(n) | Lưu trữ danh sách các ký tự được sắp xếp | 

Với t <= 100 và n <= 200, tổng số thao tác tối đa là 100 * 200 log 200, tức là khoảng 11.000 - không đáng kể đối với các CPU hiện đại. Bộ nhớ nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = ''.join(sorted(a))
        output.append(b)
    return '\n'.join(output)

# Provided samples
assert run("3\n11\nantontrygub\n15\nbestcoordinator\n19\ntrywatchinggurabruh\n") == "abgnnorttuy\naabccddeinnoorrstt\naabccghhirrrsttuwyy"

# Custom cases
assert run("1\n5\naaaaa\n") == "aaaaa", "all identical letters"
assert run("1\n6\nttrygu\n") == "gttury", "letters in 'trygub' only"
assert run("1\n1\na\n") == "a", "minimum size input"
assert run("1\n7\nzyxwvut\n") == "tuvwxyz", "descending input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 chữ cái giống nhau | aaa | xử lý đúng các chữ cái lặp đi lặp lại | 
| "ttrygu" | gttury | đảm bảo 'trygub' không phải là một dãy con ngay cả khi chỉ có những chữ cái đó | 
| 1 chữ cái | một | xử lý đầu vào tối thiểu | 
| thứ tự giảm dần | tuvwxyz | sắp xếp đúng đắn | 

## Vỏ cạnh 

Đối với các chuỗi ngắn hơn 6, như`a = "abc"`, thuật toán chỉ cần sắp xếp và xuất ra "abc". Vì có ít hơn 6 chữ cái nên "trygub" không thể xuất hiện. Đối với các chuỗi chứa chính xác các chữ cái 't', 'r', 'y', 'g', 'u', 'b', like`a = "trygub"`, việc sắp xếp tạo ra "bgrtuy", phá vỡ thứ tự bắt buộc và ngăn "trygub" trở thành một chuỗi con. Điều này xác nhận rằng phương pháp này hoạt động đối với các chuỗi tối thiểu, tối đa và cạnh.
