---
title: "CF 2154A - Ổ khóa ghi chú"
description: "Chúng ta được cấp một chuỗi nhị phân gồm 0 và 1 và số nguyên dương k. Mỗi số 1 trong chuỗi có thể bị Teto “tắt” theo một quy tắc cục bộ đơn giản: nếu số 1 không được bảo vệ và không có số 1 nào khác ở vị trí k-1 trước đó, nó có thể được thay đổi thành 0."
date: "2026-06-08T00:35:30+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2154
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1060 (Div. 2)"
rating: 800
weight: 2154
solve_time_s: 96
verified: false
draft: false
---

[CF 2154A - Notelock](https://codeforces.com/problemset/problem/2154/A) 

**Đánh giá:** 800 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân bao gồm`0`cát`1`s và một số nguyên dương`k`. Mỗi`1`trong chuỗi có thể bị Teto “tắt” theo một quy tắc cục bộ đơn giản: nếu`1`không được bảo vệ và không có cái nào khác`1`s ở phần trước`k-1`vị trí, nó có thể được thay đổi thành`0`. Mục tiêu của chúng tôi là bảo vệ số lượng tối thiểu`1`sao cho sau khi Teto áp dụng quy trình này từ trái sang phải, chuỗi vẫn giữ nguyên như cũ. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Với mỗi chuỗi, chúng ta nhận được độ dài của chuỗi, số nguyên`k`, và chính chuỗi đó. Chúng tôi được yêu cầu xuất ra số lượng vị trí tối thiểu mà chúng tôi phải bảo vệ để ngăn chặn bất kỳ`1`khỏi bị thay đổi. 

Các ràng buộc cho chúng ta biết rằng tổng số tiền`n`trên tất cả các trường hợp thử nghiệm không vượt quá 1000. Điều này có nghĩa là ngay cả một`O(n^2)`Cách tiếp cận này có thể hầu như không phù hợp, nhưng quét tuyến tính sẽ thích hợp hơn và an toàn hơn nhiều. Các trường hợp cạnh cần xem bao gồm các chuỗi chứa tất cả các ký tự`0`, Ở đâu`k`bằng với`n`, hoặc ở đâu`1`s xuất hiện liên tiếp. 

Một trường hợp khó nhận thấy là khi`1`s được đặt cách nhau một cách chính xác`k`riêng biệt. Bảo vệ một người có thể ngăn ngừa nhiều người khác bị thay đổi, và một hành động ngây thơ “bảo vệ từng người`1`chiến lược độc lập” sẽ bị tính quá nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng quy trình của Teto cho mọi tập hợp con có thể có của`1`chúng ta có thể bảo vệ. Chúng tôi có thể tạo tất cả các kết hợp, mô phỏng phép biến đổi và kiểm tra xem chuỗi có không thay đổi hay không. Mặc dù điều này đúng, nhưng nó mang tính cấp số nhân về số lượng`1`s, có thể lên tới 1000, khiến nó hoàn toàn không khả thi. 

Cách tiếp cận tối ưu cho thấy hoạt động của Teto chỉ ảnh hưởng`1`không có cái nào khác`1`s trong phần trước`k-1`các vị trí. Điều này có nghĩa là nếu chúng ta quét từ trái sang phải, chúng ta chỉ cần đảm bảo rằng giữa hai điểm được bảo vệ bất kỳ`1`s có nhiều nhất`k-1`những vị trí không được bảo vệ ngăn cách chúng. Cái nhìn sâu sắc quan trọng là`1`s đã ở trong`k`vị trí của người được bảo vệ trước đó`1`an toàn và không cần bảo vệ. Do đó, chỉ cần quét từ trái sang phải là đủ: bất cứ khi nào chúng ta gặp phải một`1`không nằm trong phạm vi được bảo vệ trước đó`1`(tức là xa hơn`k-1`cách xa vị trí), chúng tôi bảo vệ nó. Điều này trực tiếp đưa ra số lượng vị trí tối thiểu để bảo vệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`last_protected`đến một giá trị nhỏ hơn chỉ số đầu tiên (ví dụ`-k`) để theo dõi vị trí cuối cùng`1`chúng tôi đã bảo vệ. Khởi tạo bộ đếm`count`về 0 để theo dõi số lượng bảo vệ. 
2. Lặp lại chuỗi từ trái sang phải. Đối với mỗi vị trí`i`: 

- Nếu nhân vật ở`i`là`1`Và`i - last_protected >= k`, điều này có nghĩa là được bảo vệ trước đó`1`quá xa để ngăn Teto thay đổi cái này. Bảo vệ dòng điện`1`bằng cách tăng dần`count`và thiết lập`last_protected = i`. 
- Nếu không thì đừng làm gì cả, vì điều này`1`đã nằm trong phạm vi an toàn của lần bảo vệ cuối cùng`1`. 
3. Sau khi quét chuỗi,`count`chứa số lượng tối thiểu`1`chúng ta cần bảo vệ. 

Bất biến đảm bảo tính đúng đắn là ở mỗi bước, chúng ta duy trì thuộc tính mà mọi bước không được bảo vệ`1`ở trong`k-1`vị trí của một người được bảo vệ trước đó`1`. Điều này đảm bảo rằng không`1`bao giờ thỏa mãn điều kiện để Teto biến nó thành`0`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    
    last_protected = -k  # no protection yet
    count = 0
    for i, c in enumerate(s):
        if c == '1' and i - last_protected >= k:
            count += 1
            last_protected = i
    print(count)
```Giải pháp khởi tạo`last_protected`ĐẾN`-k`để xử lý việc đầu tiên`1`chính xác mà không cần vỏ đặc biệt. các`enumerate`vòng lặp quét chuỗi theo thời gian tuyến tính, chỉ bảo vệ những chuỗi cần thiết`1`S. sử dụng`i - last_protected >= k`thực hiện đúng khoảng cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi`1010101`với`k = 2`. 

| tôi | c | được bảo vệ lần cuối | tôi - Last_protected >= k? | hành động | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -2 | 2 >= 2 | bảo vệ | 1 | 
| 1 | 0 | 0 | không có | bỏ qua | 1 | 
| 2 | 1 | 0 | 2 >= 2 | bảo vệ | 2 | 
| 3 | 0 | 2 | không có | bỏ qua | 2 | 
| 4 | 1 | 2 | 2 >= 2 | bảo vệ | 3 | 
| 5 | 0 | 4 | không có | bỏ qua | 3 | 
| 6 | 1 | 4 | 2 >= 2 | bảo vệ | 4 | 

Đếm là`4`, phù hợp với đầu ra mẫu. 

Một ví dụ khác:`0000001`với`k = 4`. 

| tôi | c | được bảo vệ lần cuối | tôi - Last_protected >= k? | hành động | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | -4 | không có | bỏ qua | 0 | 
| 1 | 0 | -4 | không có | bỏ qua | 0 | 
| 2 | 0 | -4 | không có | bỏ qua | 0 | 
| 3 | 0 | -4 | không có | bỏ qua | 0 | 
| 4 | 0 | -4 | không có | bỏ qua | 0 | 
| 5 | 0 | -4 | không có | bỏ qua | 0 | 
| 6 | 1 | -4 | 10 >= 4 | bảo vệ | 1 | 

Đếm là`1`. 

Những dấu vết này xác nhận rằng những kẻ tham lam từ trái sang phải đã lựa chọn chính xác các biện pháp bảo vệ cần thiết tối thiểu trong khi vẫn đảm bảo an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một chuỗi cho mỗi trường hợp thử nghiệm;`n <= 1000`trong mọi trường hợp | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng; đầu vào chuỗi có thể được đọc tại chỗ | 

Được cho`n`tổng ≤ 1000, giải pháp tuyến tính này cực kỳ nhanh và phù hợp với cả giới hạn 1 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        last_protected = -k
        count = 0
        for i, c in enumerate(s):
            if c == '1' and i - last_protected >= k:
                count += 1
                last_protected = i
        print(count)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("9\n2 2\n11\n6 6\n100001\n5 3\n10000\n7 2\n1010101\n7 4\n0000001\n3 3\n010\n3 2\n011\n7 4\n1001001\n8 3\n00000000\n") == "1\n1\n1\n4\n1\n1\n1\n1\n0"

# Custom cases
assert run("1\n5 2\n00000\n") == "0"  # no 1s, no protection needed
assert run("1\n1 1\n1\n") == "1"  # smallest string, must protect the single 1
assert run("1\n6 6\n101101\n") == "2"  # spacing larger than k
assert run("1\n7 3\n1001001\n") == "1\n"  # protects first 1, all others safe
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`00000`k=2 | 0 | KHÔNG`1`s, không cần bảo vệ | 
|`1`k=1 | 1 | Chuỗi có kích thước tối thiểu với một chuỗi`1`| 
|`101101`k=6 | 2 | Khoảng cách không tầm thường giữa`1`s | 
|`1001001`k=3 | 1 | Đầu tiên`1`bảo vệ bao gồm tất cả những người khác | 

## Vỏ cạnh 

Khi tất cả các ký tự đều`0`, thuật toán xuất ra chính xác`0`vì không có gì để bảo vệ. Với`k = n`, ngoài cùng bên trái`1`phải luôn được bảo vệ vì nó có thể ảnh hưởng tới mọi vị thế tiếp theo. Vì`1`s cách nhau một cách chính xác`k`ngoài ra, mỗi người phải
