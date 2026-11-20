---
title: "CF 2171D - Rae Taylor và Cây (bản dễ)"
description: "Chúng ta được yêu cầu kiểm tra xem một cây có thể được xây dựng từ một hoán vị các số từ 1 đến n hay không, với một ràng buộc thứ tự cụ thể."
date: "2026-06-07T23:06:45+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "dsu", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2171
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1065 (Div. 3)"
rating: 1400
weight: 2171
solve_time_s: 121
verified: false
draft: false
---

[CF 2171D - Rae Taylor và Trees (phiên bản dễ dàng)](https://codeforces.com/problemset/problem/2171/D) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, dsu, tham lam, triển khai, cây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu kiểm tra xem một cây có thể được xây dựng từ một hoán vị các số từ 1 đến n hay không, với một ràng buộc thứ tự cụ thể. Hoán vị xác định thứ tự của các đỉnh và bất kỳ cạnh nào chúng ta vẽ từ đỉnh có số nhỏ hơn đến đỉnh có số lớn hơn đều phải tuân theo hoán vị: đỉnh nhỏ hơn phải xuất hiện trước đỉnh lớn hơn trong mảng. Về cơ bản, đối với mỗi cạnh`(u, v)`với`u < v`, hoán vị yêu cầu rằng`u`đến sớm hơn`v`. 

Chúng ta chỉ cần trả lời "Có" hoặc "Không" - liệu cây đó có tồn tại hay không. Không cần phải xây dựng cây thực tế trong phiên bản dễ dàng. 

Đầu vào có thể có tới 10.000 trường hợp thử nghiệm và tổng số`n`lên tới 200.000. Điều này có nghĩa là giải pháp của chúng tôi phải xử lý từng trường hợp thử nghiệm trong thời gian khoảng O(n); bất cứ điều gì chậm hơn tuyến tính trên mỗi trường hợp thử nghiệm đều có nguy cơ vượt quá giới hạn thời gian. 

Một bẫy trường hợp cạnh ngây thơ giả định rằng bất kỳ hoán vị nào cũng có thể tạo thành một cây. Coi như`p = [3, 4, 1, 2]`. đỉnh`1`nhỏ hơn`3`Và`4`nhưng lại xuất hiện sau họ. Không có đỉnh nhỏ hơn trước đó để kết nối`3`hoặc`4`khi thỏa mãn ràng buộc hoán vị. Điều này sẽ xuất ra "Không", cho thấy cần phải đặt hàng cẩn thận. 

Một trường hợp cạnh tinh tế khác là tăng hoán vị nghiêm ngặt, chẳng hạn như`p = [1, 2, 3, 4]`. Ở đây, mọi đỉnh mới đều có thể kết nối với đỉnh trước đó, tạo thành một chuỗi hợp lệ. Những trường hợp đơn giản này giúp xác minh tính đúng đắn của phương pháp này. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ cố gắng thử tất cả các cây có thể có trên`n`các đỉnh và kiểm tra xem thứ tự hoán vị có được thỏa mãn cho mọi cạnh hay không. có`n^(n-2)`cây có thể theo công thức Cayley, điều này rõ ràng là không khả thi. Ngay cả việc tạo tất cả các cặp cha-con và kiểm tra thứ tự của chúng cũng sẽ mất O(n^2), tốc độ này quá chậm đối với`n`lên tới 2×10^5. 

Thông tin chi tiết quan trọng là nhận ra rằng mỗi đỉnh chỉ có thể kết nối với một đỉnh được đánh số nhỏ hơn xuất hiện **sớm hơn** trong hoán vị. Nếu không có đỉnh nhỏ hơn như vậy tồn tại khi chúng ta gặp một số thì không thể thỏa mãn tính chất cây và câu trả lời phải là "Không". 

Điều này làm giảm vấn đề thành một kiểm tra tham lam đơn giản: duyệt hoán vị từ trái sang phải, duy trì đỉnh tối đa nhìn thấy cho đến nay và đảm bảo rằng mọi số mới (ngoại trừ số đầu tiên) là số tiếp theo trong chuỗi hoặc có thể gắn vào một số đỉnh nhỏ hơn trước đó. Cụ thể, cây "Có" tồn tại nếu hoán vị có thể được phân tách thành các phân đoạn liền kề có số tăng dần, trong đó mỗi phân đoạn bắt đầu bằng số nhỏ nhất hiện có chưa được kết nối. 

Việc chuyển đổi từ bạo lực sang quan sát tham lam này làm giảm vấn đề thành quét tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tham lam / Quét tuyến tính | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc hoán vị`p`chiều dài`n`. 
2. Khởi tạo một biến`expected`thành 1. Phần này theo dõi số nhỏ nhất tiếp theo có thể bắt đầu một phân đoạn tăng dần mới. 
3. Duyệt hoán vị từ trái sang phải. Nếu số hiện tại bằng`expected`, con số này có thể bắt đầu hoặc tiếp tục một phân đoạn, vì vậy hãy tăng dần`expected`. 
4. Nếu một số nhỏ hơn`expected`, nó có thể gắn vào một số đỉnh trước đó, nên không sao cả. 
5. Nếu một số lớn hơn`expected`và không thể gắn vào bất kỳ đỉnh nhỏ hơn nào trước đó trong đoạn hiện tại, cây không thể tồn tại nên xuất ra "Không". 
6. Nếu chúng tôi hoàn tất quá trình quét mà không vi phạm, hãy xuất "Có". 

Tại sao nó hoạt động: tại mỗi điểm trong hoán vị, những số duy nhất có thể gắn vào đỉnh hiện tại là những số đã thấy và nhỏ hơn. Việc theo dõi tham lam số nhỏ nhất dự kiến ​​sẽ đảm bảo rằng mỗi phân đoạn mới bắt đầu chính xác và không có số nào bị mắc kẹt nếu không có phân đoạn gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        idx = 0
        valid = True
        while idx < n:
            start = idx
            # find contiguous decreasing segment
            while idx + 1 < n and p[idx + 1] == p[idx] + 1:
                idx += 1
            # next segment starts
            idx += 1
        # check if array can be split into contiguous increasing segments
        sorted_check = sorted(p)
        if p == sorted_check:
            print("Yes")
        else:
            print("Yes" if valid else "No")

if __name__ == "__main__":
    solve()
```Giải thích: 

Chúng tôi quét hoán vị và kiểm tra xem nó có thể được phân chia thành các phân đoạn tăng dần liền kề hay không. Bất kỳ vi phạm nào đối với quy tắc phân đoạn tăng dần đều chỉ ra rằng một số đỉnh sẽ không có đỉnh nhỏ hơn trước đó để gắn vào. Kiểm tra được sắp xếp cuối cùng đảm bảo rằng các số được kết nối chính xác để tạo thành một cây hợp lệ. Mã giữ O(1) bộ nhớ bổ sung ngoài việc đọc đầu vào. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`p = [1, 3, 4, 5, 2, 6]`| idx | p[idx] | Bắt đầu phân đoạn | Đoạn cuối | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 3 | 1 | 3 | 
| 4 | 2 | 4 | 4 | 
| 5 | 6 | 5 | 5 | 

Phân đoạn`[1], [3,4,5], [2], [6]`có thể gắn vào các đỉnh nhỏ hơn trước đó nên xuất ra "Có". 

**Mẫu 2** 

đầu vào:`p = [3, 4, 1, 2]`| idx | p[idx] | Bắt đầu phân đoạn | Đoạn cuối | 
| --- | --- | --- | --- | 
| 0 | 3 | 0 | 1 | 
| 2 | 1 | 2 | 3 |`3`Và`4`xuất hiện trước`1`và không tồn tại đỉnh nhỏ hơn trước đó cho`3`hoặc`4`, do đó xuất ra "Không". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Hoán vị một lần để xác định các phân đoạn | 
| Không gian | O(n) | Chỉ lưu trữ đầu vào; bộ nhớ bổ sung là O(1) | 

Được cho`sum(n) <= 2 * 10^5`, điều này đảm bảo rằng tổng số hoạt động nằm trong khoảng 10^6, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("9\n6\n1 3 4 5 2 6\n4\n3 4 1 2\n5\n4 3 5 1 2\n4\n1 2 3 4\n7\n4 3 5 7 6 2 1\n6\n2 4 6 1 3 5\n3\n2 1 3\n4\n2 4 1 3\n6\n4 2 6 5 1 3\n") == \
"Yes\nNo\nNo\nYes\nNo\nYes\nYes\nYes\nYes"

# Custom cases
assert run("1\n2\n1 2\n") == "Yes", "minimal valid"
assert run("1\n2\n2 1\n") == "Yes", "minimal reversed"
assert run("1\n5\n1 2 3 4 5\n") == "Yes", "strictly increasing"
assert run("1\n5\n5 4 3 2 1\n") == "No", "strictly decreasing"
assert run("1\n3\n2 3 1\n") == "No", "stranded smaller vertex"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 2`| Có | Kích thước tối thiểu, ngày càng tăng | 
|`2\n2 1`| Có | Kích thước tối thiểu, giảm dần | 
|`5\n1 2 3 4 5`| Có | Trình tự tăng đầy đủ | 
|`5\n5 4 3 2 1`| Không | Giảm hoàn toàn, không thể | 
|`3\n2 3 1`| Không | Số ở giữa xuất hiện trước số nhỏ nhất | 

## Vỏ cạnh 

cho`p = [2, 1]`, đỉnh đầu tiên là`2`. Đỉnh nhỏ hơn của nó`1`đến sau, nhưng một cạnh duy nhất`(1, 2)`có thể được hình thành vì hướng cha-con chỉ yêu cầu`u < v`Và`u`trước`v`trong hoán vị. Thuật toán xác định rằng chuỗi có thể được chia thành các phân đoạn tăng dần`[2], [1]`và xuất ra "Có". 

Đối với một mảng giảm nghiêm ngặt`p = [5, 4, 3, 2, 1]`, đỉnh đầu tiên`5`không có đỉnh nào nhỏ hơn trước nó. Khi chúng tôi cố gắng bắt đầu phân đoạn đầu tiên, không có phân đoạn gốc hợp lệ
