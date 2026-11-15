---
title: "CF 1825A - LuoTianyi và chuỗi Palindrome"
description: "Chúng ta được cấp một chuỗi được đảm bảo là một chuỗi palindrome. Nhiệm vụ là tìm dãy con dài nhất của chuỗi này mà không phải là chuỗi palindrome."
date: "2026-06-09T07:36:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1825
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 872 (Div. 2)"
rating: 800
weight: 1825
solve_time_s: 73
verified: true
draft: false
---

[CF 1825A - LuoTianyi và Chuỗi Palindrome](https://codeforces.com/problemset/problem/1825/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi được đảm bảo là một chuỗi palindrome. Nhiệm vụ là tìm chuỗi con dài nhất của chuỗi này **không phải** là một bảng màu. Chuỗi con là bất kỳ chuỗi nào thu được bằng cách xóa 0 hoặc nhiều ký tự trong khi vẫn giữ thứ tự tương đối của các ký tự còn lại. Đầu ra phải là độ dài của chuỗi con này, hoặc`-1`nếu mọi dãy con không trống vẫn là một dãy palindrome. 

Các chuỗi đầu vào có độ dài lên tới 50. Vì độ dài tối đa nhỏ nên ngay cả các thuật toán có độ phức tạp thời gian bậc hai cũng khả thi, nhưng chúng ta nên hướng tới cách tiếp cận thời gian tuyến tính. Phần khó khăn là hiểu khi nào một chuỗi palindrome có thể tạo ra một chuỗi con không phải palindrome. Ví dụ, một chuỗi như`"aaa"`chỉ có các ký tự lặp lại, vì vậy bất kỳ chuỗi tiếp theo nào vẫn là một bảng màu. Mặt khác,`"ababa"`chứa ít nhất hai ký tự riêng biệt, do đó, việc loại bỏ một ký tự ở một bên có thể phá vỡ tính đối xứng và tạo ra một ký tự không phải là bảng màu. 

Các trường hợp đặc biệt thường gây khó chịu cho mọi người bao gồm các chuỗi được tạo hoàn toàn bằng một ký tự duy nhất, các chuỗi có độ dài 1 và các chuỗi là palindrome nhưng chứa nhiều ký tự riêng biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các chuỗi con không trống, kiểm tra từng chuỗi để biết thuộc tính palindrome và theo dõi độ dài tối đa trong số những chuỗi không phải là palindrome. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế: một chuỗi có độ dài 50 có$2^{50}$các chuỗi con, có giá trị lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần liệt kê các chuỗi con. Một chuỗi palindrome có tính đối xứng. Cách duy nhất để một dãy con vẫn là một dãy palindrome là nếu nó bảo toàn tính đối xứng qua tâm. Nếu chuỗi chứa ít nhất hai ký tự riêng biệt, cách đơn giản nhất để phá vỡ tính đối xứng là loại bỏ một ký tự ở cuối. Điều này đảm bảo rằng chuỗi kết quả không còn là chuỗi palindrome nữa. Do đó, độ dài của chuỗi con không palindromic dài nhất là độ dài đầy đủ của chuỗi nếu các đầu của nó khác nhau hoặc độ dài trừ đi một nếu tất cả các ký tự giống hệt nhau. 

Nói một cách đơn giản hơn, nếu tất cả các ký tự đều giống nhau thì không tồn tại dãy con không palindromic. Mặt khác, việc loại bỏ bất kỳ một ký tự nào khỏi chuỗi là đủ để phá vỡ tính đối xứng và đạt được độ dài chuỗi con không palindromic tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test, hãy đọc chuỗi palindrome`s`. 
3. Kiểm tra xem tất cả các ký tự trong`s`giống hệt nhau bằng cách so sánh tập hợp các ký tự với kích thước 1. Nếu đúng, xuất ra`-1`bởi vì bất kỳ dãy con nào cũng là một dãy palindrome. 
4. Nếu có ít nhất hai ký tự riêng biệt, xuất ra`len(s) - 1`. Việc loại bỏ một ký tự ở một trong hai đầu sẽ đảm bảo tính đối xứng bị phá vỡ và tạo ra chuỗi con không palindromic dài nhất. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: điều bất biến là một chuỗi con không phải palindromic có thể được hình thành bằng cách xóa một ký tự khỏi chuỗi chứa ít nhất hai ký tự riêng biệt. Các chuỗi có ký tự giống hệt nhau không thể tạo ra chuỗi không phải palindrome, vì vậy việc kiểm tra các ký tự hoàn toàn bằng nhau sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if len(set(s)) == 1:
        print(-1)
    else:
        print(len(s) - 1)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi chuỗi, nó chuyển đổi chuỗi đó thành một tập hợp để đếm các ký tự duy nhất. Nếu tập hợp có kích thước 1 thì chuỗi bao gồm một ký tự lặp lại duy nhất, do đó chúng ta xuất ra`-1`. Nếu không, chúng tôi xuất ra`len(s) - 1`như đã giải thích. sử dụng`set`ở đây vừa đơn giản vừa hiệu quả, đồng thời việc loại bỏ đầu vào đảm bảo không có dòng mới ở cuối. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`"abacaba"`| Bước | Hoạt động | Kết quả | 
| --- | --- | --- | 
| 1 | Chuyển đổi để đặt |`{'a','b','c'}`| 
| 2 | Kích thước của bộ > 1? | Có | 
| 3 | đầu ra`len(s) - 1`| 7 - 1 = 6 | 

Điều này cho thấy rằng việc loại bỏ một ký tự sẽ phá vỡ tính đối xứng, tạo ra chuỗi con không nhạt màu dài nhất. 

**Ví dụ 2:**`"aaa"`| Bước | Hoạt động | Kết quả | 
| --- | --- | --- | 
| 1 | Chuyển đổi để đặt |`{'a'}`| 
| 2 | Kích thước của bộ > 1? | Không | 
| 3 | đầu ra`-1`| -1 | 

Điều này thể hiện trường hợp cạnh trong đó tất cả các ký tự giống hệt nhau và không tồn tại chuỗi con không palindromic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tạo một bộ và đo chiều dài theo tỷ lệ tuyến tính với chiều dài chuỗi | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Bộ ký tự có thể chứa tối đa n ký tự duy nhất | 

Với các ràng buộc (tối đa n = 50 và t = 1000), thời gian trong trường hợp xấu nhất là khoảng 50.000 thao tác, tức là trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if len(set(s)) == 1:
            print(-1)
        else:
            print(len(s) - 1)
    return output.getvalue().strip()

# provided samples
assert run("4\nabacaba\naaa\ncodeforcesecrofedoc\nlol\n") == "6\n-1\n18\n2"

# custom cases
assert run("2\na\naaa\n") == "-1\n-1", "minimum-size and all equal"
assert run("1\nabcdeedcba\n") == "9", "palindrome with distinct characters"
assert run("1\nzzzzzzzzzz\n") == "-1", "all equal max length"
assert run("1\nabba\n") == "3", "even-length palindrome with two distinct"
assert run("1\nab\n") == "1", "length 2 palindrome with distinct chars"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`-1`| Chuỗi ký tự đơn | 
|`"abcdeedcba"`|`9`| Xóa một ký tự khỏi bảng màu có nhiều ký tự riêng biệt | 
|`"zzzzzzzzzz"`|`-1`| Chuỗi dài các ký tự giống hệt nhau | 
|`"abba"`|`3`| Bảng màu có độ dài chẵn với các ký tự riêng biệt | 
|`"ab"`|`1`| Palindrome không tầm thường ngắn nhất | 

## Vỏ cạnh 

Đối với chuỗi`"a"`, tập hợp chứa một phần tử duy nhất, vì vậy đầu ra là`-1`. Không có dãy số nào có thể phá vỡ tính đối xứng. Vì`"aaaaa"`, tất cả các ký tự đều giống hệt nhau, do đó kích thước được đặt là 1 và đầu ra là`-1`. Đối với một chuỗi như`"ab"`, loại bỏ một trong hai`a`hoặc`b`sản xuất`"b"`hoặc`"a"`, vẫn là palindrome, vì vậy độ dài chuỗi con không palindrome dài nhất là`len(s)-1 = 1`, phù hợp với quy luật chung. Những lần kiểm tra này xác nhận thuật toán xử lý chính xác kích thước đầu vào tối thiểu và tối đa, các ký tự lặp lại và các bảng màu nhỏ.
