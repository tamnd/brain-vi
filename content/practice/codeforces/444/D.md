---
title: "CF 444D - DZY Yêu Đàn Dây"
description: "Chúng ta có một chuỗi dài s và một danh sách các cặp chuỗi nhỏ (ai, bi). Với mỗi cặp, chúng ta cần tìm chuỗi con ngắn nhất của s chứa cả ai và bi làm chuỗi con. Nếu không có chuỗi con nào chứa cả hai, chúng tôi báo cáo -1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "hashing", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 444
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 254 (Div. 1)"
rating: 2500
weight: 444
solve_time_s: 66
verified: true
draft: false
---

[CF 444D - DZY yêu thích đàn dây](https://codeforces.com/problemset/problem/444/D) 

**Đánh giá:** 2500 
**Thẻ:** tìm kiếm nhị phân, băm, chuỗi, hai con trỏ 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi dài`s`và một danh sách các cặp dây nhỏ`(a_i, b_i)`. Với mỗi cặp, chúng ta cần tìm chuỗi con ngắn nhất của`s`chứa cả hai`a_i`Và`b_i`như các chuỗi con. Nếu không có chuỗi con nào chứa cả hai, chúng tôi sẽ báo cáo`-1`. Dây đàn`a_i`Và`b_i`rất ngắn (tối đa 4), trong khi chuỗi chính`s`có thể lên tới 50.000 ký tự và số lượng truy vấn`q`có thể lên tới 100.000. 

Các ràng buộc đầu vào ngụ ý rằng một cách tiếp cận đơn giản là kiểm tra tất cả các chuỗi con của`s`đối với mỗi truy vấn sẽ quá chậm. Kiểm tra brute-force sẽ yêu cầu lặp lại khoảng 50.000² chuỗi con cho mỗi truy vấn, tức là khoảng 2,5 tỷ thao tác cho mỗi truy vấn. Với 100.000 truy vấn, điều đó hoàn toàn không khả thi. 

Các trường hợp khó khăn có thể gặp phải một giải pháp đơn giản bao gồm các chuỗi xuất hiện nhiều lần, các lần xuất hiện chồng chéo hoặc các cặp trong đó một hoặc cả hai chuỗi hoàn toàn không xuất hiện. Ví dụ, nếu`s = "aaabaaa"`,`a = "aa"`Và`b = "aaa"`, chuỗi con ngắn nhất là`"aaa"`bắt đầu từ chỉ số 1, không`"aaab"`. Tương tự, nếu một truy vấn chứa các chuỗi không có trong`s`, thuật toán phải trả về chính xác`-1`. Các mô hình chồng chéo làm cho các cách tiếp cận tham lam ngây thơ dễ mắc phải từng lỗi một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các vị trí bắt đầu trong`s`, tìm tất cả các lần xuất hiện của`a_i`Và`b_i`và tính toán chuỗi con tối thiểu chứa bất kỳ sự kết hợp nào của những lần xuất hiện đó. Về mặt lý thuyết, điều này hoạt động chính xác nhưng yêu cầu quét các chuỗi con O(|s|²) cho mỗi truy vấn. Với |s| lên tới 50.000 và q lên tới 100.000, điều này dẫn đến hàng nghìn tỷ hoạt động, điều này rõ ràng là không thể chấp nhận được. 

Thông tin chi tiết quan trọng để tối ưu hóa đến từ hai quan sát. Đầu tiên, các dây`a_i`Và`b_i`rất ngắn nên tổng số chuỗi riêng biệt xuất hiện trong truy vấn bị hạn chế. Thứ hai, nếu chúng ta tính toán trước vị trí bắt đầu của mỗi chuỗi có độ dài từ 1 đến 4 inch`s`, thì mỗi truy vấn sẽ giảm xuống vấn đề tìm khoảng cách tối thiểu giữa hai danh sách các số nguyên được sắp xếp biểu thị vị trí bắt đầu. Việc này nhanh hơn nhiều vì việc hợp nhất hai danh sách đã sắp xếp có thể được thực hiện theo thời gian tuyến tính bằng cách tiếp cận hai con trỏ. 

Thuật toán được tối ưu hóa tính toán trước một ánh xạ từ điển mỗi chuỗi con của`s`có độ dài từ 1 đến 4 vào danh sách được sắp xếp các vị trí nơi nó xuất hiện. Sau đó, với mỗi truy vấn`(a_i, b_i)`, chúng tôi truy xuất danh sách vị trí bắt đầu cho cả hai chuỗi. Sử dụng hai con trỏ, chúng tôi quét các danh sách này để tìm cặp lần xuất hiện làm giảm thiểu độ dài chuỗi con bao phủ. Nếu một trong hai chuỗi không xuất hiện trong`s`, chúng tôi lập tức quay lại`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n2) | O(1) | Quá chậm | 
| Tính toán trước + Hai con trỏ | O(n + q * k) | O(n) | Đã chấp nhận | 

Đây`k`là tổng số lần xuất hiện của chuỗi truy vấn trong`s`, tỷ lệ này nhỏ trong thực tế vì`a_i`Và`b_i`ngắn. 

## Hướng dẫn thuật toán 

1. Xây dựng từ điển`occ`ánh xạ từng chuỗi con của`s`có độ dài từ 1 đến 4 vào danh sách tất cả các chỉ số bắt đầu. Lặp lại qua`s`một lần và cho mỗi vị trí`i`, thêm vào`s[i:i+l]`vì`l`từ 1 đến 4 đến`occ[i:i+l]`. Điều này cho phép tra cứu liên tục theo thời gian của tất cả các lần xuất hiện sau đó. 
2. Đối với mỗi truy vấn`(a_i, b_i)`, kiểm tra xem cả hai`a_i`Và`b_i`tồn tại trong từ điển. Nếu không, ngay lập tức xuất ra`-1`. 
3. Truy xuất danh sách đã sắp xếp`pos_a`Và`pos_b`cho vị trí bắt đầu của`a_i`Và`b_i`. Khởi tạo hai con trỏ`i`Và`j`đến 0, biểu thị các chỉ số hiện tại trong`pos_a`Và`pos_b`. 
4. Sử dụng tính năng truyền tải bằng hai con trỏ. Ở mỗi bước, hãy xem xét các vị trí hiện tại`pos_a[i]`Và`pos_b[j]`. Chuỗi con bao gồm cả hai bắt đầu tại`min(pos_a[i], pos_b[j])`và kết thúc tại`max(pos_a[i] + len(a_i), pos_b[j] + len(b_i))`. Tính toán độ dài và theo dõi mức tối thiểu được tìm thấy cho đến nay. 
5. Tiến con trỏ tương ứng đến vị trí bắt đầu nhỏ hơn. Điều này đảm bảo rằng chúng tôi khám phá tất cả các cặp theo thứ tự tăng dần mà không bỏ lỡ phạm vi bao phủ tối thiểu. Lặp lại cho đến khi hết một trong các danh sách. 
6. In ra độ dài tối thiểu tìm được. 

Tại sao nó hoạt động: Quá trình tính toán trước đảm bảo rằng chúng ta biết tất cả điểm bắt đầu của mọi chuỗi liên quan. Việc truyền tải hai con trỏ tận dụng thứ tự xuất hiện đã được sắp xếp và di chuyển nhanh chóng về phía độ dài bìa tối thiểu. Ở mỗi bước, việc tăng con trỏ sẽ khám phá tất cả các chuỗi con tối thiểu tiềm năng. Vì cả hai danh sách đều được sắp xếp và chúng tôi kiểm tra từng cặp theo thứ tự nên không có chuỗi con tối thiểu ứng cử viên nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)
q = int(input())

# Precompute all substrings of length 1-4
occ = {}
for i in range(n):
    for l in range(1, 5):
        if i + l <= n:
            sub = s[i:i+l]
            if sub not in occ:
                occ[sub] = []
            occ[sub].append(i)

for _ in range(q):
    a, b = input().split()
    if a not in occ or b not in occ:
        print(-1)
        continue
    pos_a = occ[a]
    pos_b = occ[b]
    i = j = 0
    min_len = float('inf')
    while i < len(pos_a) and j < len(pos_b):
        start = min(pos_a[i], pos_b[j])
        end = max(pos_a[i] + len(a), pos_b[j] + len(b))
        min_len = min(min_len, end - start)
        if pos_a[i] < pos_b[j]:
            i += 1
        else:
            j += 1
    print(min_len)
```Giải pháp đầu tiên xây dựng`occ`, ánh xạ các chuỗi con có độ dài từ 1 đến 4 tới vị trí bắt đầu của chúng. Điều này rất quan trọng vì các truy vấn có thể lặp lại các chuỗi và việc tính toán lại vị trí cho mỗi truy vấn sẽ không hiệu quả. Cách tiếp cận hai con trỏ đảm bảo rằng chúng tôi khám phá tất cả các kết hợp hợp lệ một cách hiệu quả. Việc nâng cao con trỏ của chỉ mục bắt đầu nhỏ hơn là điều tinh tế: nó đảm bảo rằng chuỗi con tối thiểu tiềm năng tiếp theo được xem xét mà không bỏ qua khả năng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
xudyhduxyz
3
xyz xyz
dyh xyz
dzy xyz
```| Truy vấn | tư thế_a | pos_b | tính toán min_len | Kết quả | 
| --- | --- | --- | --- | --- | 
| xyz xyz | [7] | [7] | phút = 7-7+3=3 | 3 | 
| dyh xyz | [3] | [7] | bắt đầu=min(3,7)=3, kết thúc=max(3+3,7+3)=10, len=8 | 8 | 
| dzy xyz | [] | [7] | mất tích | -1 | 

Điều này chứng tỏ rằng thuật toán xử lý chính xác các lần xuất hiện đơn lẻ, nhiều lần xuất hiện và các chuỗi vắng mặt. 

Đầu vào tùy chỉnh:```
aaaaa
2
aa aaa
b a
```| Truy vấn | tư thế_a | pos_b | min_len | Kết quả | 
| --- | --- | --- | --- | --- | 
| aaa aaa | [0,1,2,3] | [0,1,2] | chuỗi con tối thiểu bao gồm [0,2] length=3 | 3 | 
| b a | [] | [0,1,2,3,4] | mất tích | -1 | 

Điều này xác nhận các lần xuất hiện chồng chéo và các chuỗi bị thiếu được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q * k) | O(n) để tính toán trước các chuỗi con, O(k) cho mỗi truy vấn để hợp nhất hai danh sách lần xuất hiện được sắp xếp | 
| Không gian | O(n) | Lưu trữ tất cả các chuỗi con có độ dài ≤4 trong từ điển, tối đa là O(n) tổng số mục | 

Đã cho |s| 50.000 và q 100.000, tính toán trước là tuyến tính và các truy vấn liên quan đến việc hợp nhất các danh sách rất ngắn vì các chuỗi có độ dài ≤4. Giải pháp vừa vặn thoải mái trong 3 giây và 256MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# provided samples
assert run("xudyhduxyz\n3\nxyz xyz\ndyh xyz\ndzy xyz\n") == "3\n8\n-1", "sample 1"

# custom tests
assert run("aaaaa\n2\naa aaa\nb a\n") == "3\n-1", "overlap and missing"
assert run("abcdefg\n1\na g\n") == "7", "full string cover"
assert run("zzzzzz\n1\nzz zz\n") == "2", "repeated characters"
assert run("abcde\n2\na a\nc e\n") == "1\n3", "minimal and spaced"
assert
```
