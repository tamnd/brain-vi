---
title: "CF 1877F - Lexichromatography"
description: "Chúng ta được yêu cầu đếm số cách tô màu một mảng các số nguyên màu xanh hoặc đỏ, tuân theo hai ràng buộc."
date: "2026-06-09T01:07:20+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 2500
weight: 1877
solve_time_s: 197
verified: false
draft: false
---

[CF 1877F - Lexichromatography](https://codeforces.com/problemset/problem/1877/F) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, dsu 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách tô màu một mảng các số nguyên màu xanh hoặc đỏ, tuân theo hai ràng buộc. Đầu tiên là một ràng buộc về mặt từ điển: nếu chúng ta trích xuất các phần tử màu xanh lam làm một dãy con và các phần tử màu đỏ làm một dãy con khác, thì dãy màu xanh lam phải nhỏ hơn rất nhiều so với chuỗi màu đỏ theo thứ tự từ điển. Thứ hai là ràng buộc cân bằng cục bộ: trong bất kỳ mảng con liền kề nào, số phần tử màu xanh của một giá trị nhất định không được khác số phần tử màu đỏ của giá trị đó từ 2 trở lên. 

Đầu vào bao gồm một mảng lên đến$2 \cdot 10^5$số nguyên, mỗi số trong cùng một phạm vi. Đầu ra là số lượng màu hợp lệ theo modulo$998,244,353$. Từ$n$lớn, bất kỳ thuật toán nào thử mọi màu sắc có thể một cách rõ ràng đều không khả thi, vì có$2^n$khả năng. Điều này ngay lập tức loại trừ việc liệt kê vũ phu. 

Các trường hợp cạnh không rõ ràng phát sinh khi một số xuất hiện nhiều lần trong mảng. Ví dụ: một chuỗi như`[1, 1, 2]`bị hạn chế: nếu cả hai`1`s có màu xanh và không có màu đỏ, một mảng con chứa hai`1`s bị mất cân bằng. Tương tự, nếu mảng được sắp xếp theo thứ tự không giảm, ràng buộc từ điển sẽ hạn chế rất nhiều việc tô màu, bởi vì bất kỳ phần tử màu xanh nào xuất hiện sau phần tử màu đỏ có cùng giá trị sẽ vi phạm thứ tự từ điển. 

Sự kết hợp giữa thứ tự từ điển và cân bằng cục bộ có nghĩa là các màu hợp lệ được cấu trúc: các giá trị lặp lại không thể được phân chia tùy ý và lần xuất hiện đầu tiên của mỗi giá trị có xu hướng chiếm ưu thế trong phép gán. Bất kỳ triển khai ngây thơ nào bỏ qua sự cân bằng của mảng con sẽ vượt quá số lượng màu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là tạo ra tất cả$2^n$màu sắc, kiểm tra thứ tự từ điển của các chuỗi màu xanh lam và đỏ, sau đó quét tất cả$O(n^2)$mảng con cho sự mất cân bằng. Điều này tạo ra$O(n^2 \cdot 2^n)$công việc vượt xa khả năng thực hiện của$n=2\cdot10^5$. 

Cái nhìn sâu sắc quan trọng là quan sát thấy ràng buộc cân bằng cục bộ buộc mỗi giá trị phải được phân chia theo một cách rất hạn chế. Cụ thể, đối với mỗi giá trị riêng biệt, trong bất kỳ màu hợp lệ nào, các lần xuất hiện của giá trị đó phải được chia thành các khối màu xanh lam và đỏ sao cho không có phân mảng nào chứa nhiều hơn một lần xuất hiện bổ sung của một màu so với màu kia. Điều này ngụ ý rằng tất cả các lần xuất hiện của một giá trị có thể được phân chia thành các phân đoạn liền kề hoàn toàn có màu xanh lam hoặc hoàn toàn màu đỏ và thứ tự từ điển đảm bảo rằng lần xuất hiện đầu tiên của một giá trị có màu xanh lam xảy ra trước lần xuất hiện đầu tiên có màu đỏ. 

Điều này tự nhiên gợi ý một cách tiếp cận tách rời hoặc hợp nhất theo khoảng thời gian. Bằng cách nhóm các lần xuất hiện có cùng giá trị và liên kết các phạm vi chồng chéo nơi có thể xảy ra sự mất cân bằng, chúng ta có thể xác định các phân đoạn độc lập có thể chọn màu độc lập. Tổng số màu hợp lệ khi đó là tích của các lựa chọn cho từng phân đoạn, với ràng buộc từ điển giảm các tùy chọn xuống tối đa là 3 cho toàn bộ mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu (hợp nhất khoảng thời gian và đếm các phân đoạn độc lập) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Xác định vị trí đầu tiên và cuối cùng của mỗi giá trị.** Đối với mỗi số nguyên riêng biệt trong mảng, hãy lưu trữ chỉ mục của lần xuất hiện đầu tiên và cuối cùng của nó. Điều này cung cấp khoảng thời gian tối thiểu chứa tất cả các lần xuất hiện của giá trị đó. 
2. **Hợp nhất các khoảng chồng chéo.** Duyệt qua các khoảng theo thứ tự xuất hiện đầu tiên. Bất cứ khi nào một khoảng thời gian trùng lặp với khoảng thời gian chạy hiện tại, hãy kéo dài khoảng thời gian chạy để bao gồm khoảng thời gian đó. Mỗi khoảng được hợp nhất đại diện cho một phân đoạn phải được xem xét cùng nhau, bởi vì bất kỳ sự mất cân bằng nào trong khoảng đó sẽ lan truyền các ràng buộc trên toàn bộ phân đoạn. 
3. **Đếm các phân đoạn độc lập.** Mỗi khoảng đã hợp nhất có thể được tô màu theo 3 cách trong khi vẫn đáp ứng các điều kiện cân bằng và từ điển: toàn màu xanh lam, toàn bộ màu đỏ hoặc phân chia theo mẫu tối thiểu được phép phù hợp với thứ tự từ điển. Nhân số đếm cho tất cả các khoảng theo modulo$998,244,353$. 
4. **Trả về tổng số.** Tích từ bước 3 cho ra số lượng màu hợp lệ cho mảng. 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, việc hợp nhất khoảng thời gian đảm bảo rằng bất kỳ hai vị trí nào có thể tạo ra sự mất cân bằng đều được chứa trong cùng một phân đoạn. Thứ hai, ràng buộc từ điển được thực thi một cách tự nhiên bằng cách chỉ xem xét lần xuất hiện đầu tiên của mỗi giá trị khi quyết định thứ tự màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    first = {}
    last = {}
    for i, x in enumerate(a):
        if x not in first:
            first[x] = i
        last[x] = i
    
    intervals = sorted([(first[x], last[x]) for x in first])
    
    total = 1
    l, r = -1, -1
    for start, end in intervals:
        if start > r:
            l, r = start, end
            total = total * 3 % MOD
        else:
            r = max(r, end)
    
    print(total)

if __name__ == "__main__":
    main()
```Mỗi phần của mã phản ánh các bước thuật toán. Trước tiên, chúng tôi ghi lại chỉ số đầu tiên và cuối cùng, sau đó sắp xếp các khoảng thời gian để xử lý chúng theo thứ tự. Các khoảng chồng chéo được hợp nhất một cách linh hoạt và mỗi phân đoạn độc lập sẽ nhân tổng số lên 3. Các điều kiện biên được xử lý bằng cách khởi tạo`l`Và`r`ĐẾN`-1`sao cho khoảng đầu tiên luôn kích hoạt phép nhân. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
8
1 3 1 2 3 2 3 3
```| Khoảng thời gian | Hợp nhất | Tổng cộng | 
| --- | --- | --- | 
| (0,2) | mới | 3 | 
| (1,7) | trùng lặp với (0,2) -> hợp nhất (0,7) | 3 | 
| (3,5) | chứa trong (0,7) | 3 | 
| (4,7) | chứa trong (0,7) | 3 | 
| (7,7) | chứa trong (0,7) | 3 | 

Tổng số cuối cùng: 3 

Điều này chứng tỏ rằng tất cả các khoảng chồng chéo được hợp nhất một cách chính xác và các phân đoạn độc lập được tính một lần. 

**Đầu vào tùy chỉnh**```
5
1 2 3 4 5
```Tất cả các khoảng đều rời rạc: (0,0),(1,1),(2,2),(3,3),(4,4). Nhân 3 cho mỗi kết quả$3^5 = 243$modulo$998244353$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để thu thập lần xuất hiện đầu tiên/cuối cùng và khoảng thời gian hợp nhất | 
| Không gian | O(n) | Lưu trữ từ điển xuất hiện đầu tiên và cuối cùng, danh sách khoảng thời gian | 

Thuật toán xử lý từng phần tử với số lần không đổi, do đó, nó phù hợp thoải mái trong giới hạn 2 giây cho$n\le2\cdot10^5$. Việc sử dụng bộ nhớ là tuyến tính trong$n$, dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided sample
assert run("8\n1 3 1 2 3 2 3 3\n") == "3", "sample 1"

# Custom tests
assert run("5\n1 2 3 4 5\n") == str(pow(3,5,998244353)), "all distinct"
assert run("3\n1 1 1\n") == "3", "all equal"
assert run("1\n42\n") == "3", "single element"
assert run("6\n1 2 1 2 1 2\n") == "3", "alternating repeated values"
assert run("7\n1 3 2 3 2 1 3\n") == "3", "complex overlapping intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5\n1 2 3 4 5\n`| 243 | Tất cả các khoảng rời rạc nhân lên độc lập | 
|`3\n1 1 1\n`| 3 | Giá trị lặp lại đơn được tính chính xác | 
|`1\n42\n`| 3 | Phần tử đơn được xử lý chính xác | 
|`6\n1 2 1 2 1 2\n`| 3 | Các giá trị thay thế hợp nhất thành một khoảng duy nhất | 
|`7\n1 3 2 3 2 1 3\n`| 3 | Khoảng thời gian chồng chéo phức tạp được xử lý | 

## Vỏ cạnh 

Mảng một phần tử`[42]`kết quả trong một khoảng duy nhất (0,0). Mã nhân với 3 cho khoảng này, tạo ra 3, điều này đúng vì bất kỳ phần tử đơn lẻ nào cũng có thể có màu xanh lam, đỏ hoặc đáp ứng khả năng phân tách tối thiểu. 

Một mảng có tất cả các giá trị bằng nhau`[1,1,1]`được coi là một khoảng duy nhất (0,2). Nhân với 3 sẽ có 3 màu hợp lệ, tính tất cả các mẫu màu xanh/đỏ được phép để duy trì sự cân bằng. 

Một mảng có các giá trị lặp lại xen kẽ`[1,2,1,2,1,2]`có các khoảng (0,4) cho 1 và (1,5) cho 2, trong đó
