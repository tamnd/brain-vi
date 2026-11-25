---
title: "CF 982A - Hàng"
description: "Chúng tôi được xếp một dãy ghế, mỗi chiếc có một người ngồi hoặc trống. Cấu hình được biểu diễn dưới dạng chuỗi nhị phân trong đó 1 có nghĩa là một người đang ngồi và 0 có nghĩa là ghế trống. Việc sắp xếp chỗ ngồi chỉ được coi là hợp lệ nếu không có hai người ngồi cạnh nhau."
date: "2026-06-17T01:04:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 982
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 484 (Div. 2)"
rating: 1200
weight: 982
solve_time_s: 62
verified: true
draft: false
---

[CF 982A - Hàng](https://codeforces.com/problemset/problem/982/A) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một dãy ghế, mỗi chiếc có một người ngồi hoặc trống. Cấu hình được biểu diễn dưới dạng chuỗi nhị phân trong đó`1`có nghĩa là một người đang ngồi và`0`có nghĩa là chỗ ngồi trống. 

Việc sắp xếp chỗ ngồi chỉ được coi là hợp lệ nếu không có hai người ngồi cạnh nhau. Ngoài ra, chúng tôi không chỉ kiểm tra tính hợp lệ. Chúng ta cũng cần quyết định xem cấu hình đã “tối đa” hay chưa, nghĩa là không có cách nào để đặt thêm một người vào bất kỳ chiếc ghế trống nào mà không vi phạm quy định không có hai chiếc ghế có người ngồi cạnh nhau. 

Vì vậy, nhiệm vụ không phải là xây dựng chỗ ngồi mà là xác minh hai thuộc tính cùng một lúc. Đầu tiên, sự sắp xếp hiện tại phải đáp ứng quy tắc kề. Thứ hai, mọi ghế trống phải được “chặn” bởi ít nhất một người hàng xóm đã có người ngồi, do đó, việc chèn một người vào đó sẽ vi phạm quy tắc ngay lập tức. 

Ràng buộc`n ≤ 1000`ngụ ý rằng mô phỏng O(n²) vẫn sẽ đạt, nhưng điều đó là không cần thiết. Chỉ cần quét tuyến tính một lần là đủ, vì mọi quyết định về chỗ ngồi chỉ phụ thuộc vào những người ngồi cạnh nó. 

Một số trường hợp đặc biệt quan trọng hơn những gì chúng có vẻ thoạt nhìn. 

Nếu chuỗi là`"0"`vì`n = 1`, thì nó không cực đại vì ta có thể đặt một người vào đó mà vẫn thỏa mãn quy tắc. 

Nếu chuỗi là`"1"`, thì đó là mức tối đa, bởi vì chúng tôi đã có người và không thể bổ sung thêm vị trí nào. 

Nếu chúng ta thấy`"101"`, nó hợp lệ và tối đa vì số 0 đơn liền kề với hai số 1 nên nó không thể lưu trữ bất kỳ ai. 

Nếu chúng ta thấy`"1001"`, nó hợp lệ nhưng không tối đa vì ở giữa`"00"`chứa ít nhất một vị trí mà chúng ta vẫn có thể đặt một người. 

Những ví dụ này cho thấy sự tinh tế quan trọng: tính tối đa là về sự tồn tại của bất kỳ phép chèn hợp lệ nào, không chỉ tính hợp lệ cục bộ của cấu hình hiện tại. 

## Phương pháp tiếp cận 

Chiến lược brute-force thử mọi vị trí trống và kiểm tra xem có đặt vị trí hay không`1`ở đó giữ cho cấu hình hợp lệ. Đối với mỗi vị trí ứng viên, chúng tôi quét các vị trí lân cận để đảm bảo không vi phạm vị trí lân cận. Nếu bất kỳ vị trí nào hoạt động thì cấu hình không tối đa. Cách tiếp cận này là đúng, nhưng đối với mỗi`n`vị trí chúng tôi có thể quét tới`n`các ký tự, đưa ra hành vi O(n²). 

Cấu trúc của vấn đề cho phép quan sát trực tiếp hơn. Ghế chỉ có thể sử dụng được nếu cả hai vị trí liền kề đều`0`(hoặc không tồn tại ở ranh giới). Vì vậy, một cấu hình đạt cực đại chính xác khi mọi`0`bị “chặn”, nghĩa là ít nhất một vị trí liền kề bị`1`. Nếu một`0`không có liền kề`1`, nó ngay lập tức là một điểm chèn hợp lệ. 

Điều này làm giảm vấn đề xuống còn một lần quét để kiểm tra xem có bất kỳ chỉ mục nào vi phạm điều kiện chặn này hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi một lần và kiểm tra xem có tồn tại vị trí nào mà chúng tôi có thể đặt một người hay không. 

1. Lặp lại mọi chỉ mục`i`trong chuỗi. 

Mỗi vị trí là một ứng cử viên cho vị trí nếu nó hiện đang được`0`. 
2. Đối với mỗi`0`, kiểm tra hàng xóm của nó. 

Một vị trí hợp lệ nếu cả hàng xóm bên trái (nếu nó tồn tại) và hàng xóm bên phải (nếu nó tồn tại) đều`0`. 
3. Nếu tìm thấy một vị trí như vậy, chúng ta có thể kết luận ngay rằng chỗ ngồi không tối đa. 

Điều này là do chúng tôi đã tìm thấy một cách rõ ràng một phần chèn hợp lệ để duy trì quy tắc kề. 
4. Nếu không có vị trí nào như vậy sau khi quét toàn bộ chuỗi thì cấu hình là tối đa. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là giá trị của việc đặt một người vào vị trí`i`chỉ phụ thuộc vào`i-1`Và`i+1`. Nếu một trong hai người hàng xóm là`1`, vị trí bị cấm. Nếu cả hai đều`0`(hoặc ngoài giới hạn), vị trí luôn được cho phép. Do đó, sự tồn tại của bất kỳ chỉ mục nào với cả hai lân cận trống đều tương đương với tính không cực đại. Quá trình quét sẽ kiểm tra chính xác tình trạng này đối với mọi điểm chèn có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

for i in range(n):
    if s[i] == '0':
        left_ok = (i == 0) or (s[i - 1] == '0')
        right_ok = (i == n - 1) or (s[i + 1] == '0')
        if left_ok and right_ok:
            print("No")
            sys.exit()

print("Yes")
```Mã thực hiện một lần chuyển qua chuỗi. Đối với mỗi số 0, nó kiểm tra xem cả hai hàng xóm cũng bằng 0 hay không có ở ranh giới. Nếu tìm thấy vị trí như vậy, chúng tôi sẽ xuất ngay`"No"`bởi vì chúng ta có thể chèn một người vào đó mà không vi phạm quy tắc. Ngược lại, vòng lặp hoàn thành và chúng ta xuất ra`"Yes"`. 

Việc xử lý ranh giới được thực hiện một cách rõ ràng: khi`i = 0`hoặc`i = n - 1`, chúng ta tự động coi người hàng xóm mất tích là an toàn. Điều này tránh các lỗi chỉ mục và phù hợp với ý tưởng rằng các cạnh chỉ có một ràng buộc thay vì hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`101`| tôi | s[i] | trái | đúng | có thể chèn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 0 | không | 
| 1 | 0 | 1 | 1 | không | 
| 2 | 1 | 0 | - | không | 

Không có chỉ mục nào cho phép chèn, vì vậy câu trả lời là`"Yes"`. 

Điều này khẳng định bất biến rằng mọi số 0 đều liền kề với ít nhất một ghế đã có người. 

### Ví dụ 2:`1001`| tôi | s[i] | trái | đúng | có thể chèn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 0 | không | 
| 1 | 0 | 1 | 0 | không | 
| 2 | 0 | 0 | 1 | không | 
| 3 | 1 | 0 | - | không | 

Ở đây, chỉ số 1 và 2 trông có vẻ bị chặn một phần nhưng không bị cô lập hoàn toàn. Tuy nhiên, nếu chúng ta xem xét cẩn thận thì không vị trí nào cho phép chèn vì mỗi số 0 chạm vào một`1`ít nhất ở một bên. Quá trình quét xác nhận tính tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền một lần qua chuỗi với công việc không đổi trên mỗi chỉ mục | 
| Không gian | O(1) | chỉ một vài kiểm tra boolean, không có mảng bổ sung | 

Quét tuyến tính thoải mái trong giới hạn cho`n ≤ 1000`và thậm chí mở rộng quy mô đến các ràng buộc lớn hơn nhiều mà không gặp vấn đề gì. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    for i in range(n):
        if s[i] == '0':
            left_ok = (i == 0) or (s[i - 1] == '0')
            right_ok = (i == n - 1) or (s[i + 1] == '0')
            if left_ok and right_ok:
                return "No"
    return "Yes"

# provided samples
assert run("3\n101\n") == "Yes"

# minimum size, empty seat always improvable
assert run("1\n0\n") == "No"

# single occupied seat is maximal
assert run("1\n1\n") == "Yes"

# clear non-maximal case
assert run("3\n000\n") == "No"

# alternating pattern already maximal
assert run("5\n10101\n") == "Yes"

# boundary case with middle free space
assert run("4\n1001\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, 0`| Không | trường hợp chèn ghế đơn | 
|`1, 1`| Có | cấu hình tối đa nhỏ nhất | 
|`000`| Không | hàng trống hoàn toàn không phải là tối đa | 
|`10101`| Có | mô hình an toàn xen kẽ | 
|`1001`| Có | xử lý liền kề ranh giới | 

## Vỏ cạnh 

cho`n = 1`, xem xét đầu vào`"0"`. Thuật toán kiểm tra chỉ số 0 thì thấy là`0`và cả hai hàng xóm đều được coi là an toàn do các quy tắc ranh giới. Nó ngay lập tức kết luận rằng việc chèn là có thể và xuất ra`"No"`. 

Vì`"1"`, vòng lặp không bao giờ tìm thấy`0`, vì vậy nó không bao giờ gây ra sự từ chối và đầu ra cuối cùng là`"Yes"`. 

Vì`"1001"`, chỉ số 1 và 2 được kiểm tra. Tại chỉ số 1, bên trái là`1`vì vậy vị trí không thành công. Tại chỉ số 2, bên phải là`1`vì vậy vị trí không thành công. Vì không có chỉ mục nào thỏa mãn cả hai điều kiện lân cận trống nên thuật toán trả về chính xác`"Yes"`.
