---
title: "CF 1216A - Tiền tố"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự a và b và độ dài của nó được đảm bảo là chẵn. Nhiệm vụ là chuyển đổi chuỗi này bằng cách sử dụng số lần lật ký tự đơn tối thiểu sao cho mọi tiền tố có độ dài chẵn đều chứa chính xác cùng số a và…"
date: "2026-06-15T18:43:12+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1216
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 587 (Div. 3)"
rating: 800
weight: 1216
solve_time_s: 158
verified: false
draft: false
---

[CF 1216A - Tiền tố](https://codeforces.com/problemset/problem/1216/A) 

**Đánh giá:** 800 
**Thẻ:** chuỗi 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân chỉ gồm các ký tự`a`Và`b`và độ dài của nó được đảm bảo bằng nhau. Nhiệm vụ là biến đổi chuỗi này bằng cách sử dụng số lần lật ký tự đơn tối thiểu sao cho mọi tiền tố có độ dài chẵn đều chứa chính xác cùng một số lượng ký tự.`a`Và`b`. 

Một cách hữu ích để diễn đạt lại yêu cầu là quét chuỗi từ trái sang phải và xem các tiền tố có độ dài 2, 4, 6, v.v. Mỗi tiền tố như vậy phải được cân bằng hoàn hảo, nghĩa là một nửa số ký tự của nó được`a`và nửa còn lại là`b`. 

Hạn chế chính là chúng ta được phép lật bất kỳ vị trí nào một cách độc lập, xoay`a`vào trong`b`hoặc ngược lại và chúng tôi muốn giảm thiểu số lần lật cần thiết. 

Vì độ dài chuỗi có thể lên tới 200.000 nên mọi giải pháp đều phải chạy theo thời gian tuyến tính. Bất cứ điều gì liên tục tính toán lại số liệu thống kê tiền tố hoặc thử kết hợp các sửa đổi cho mỗi tiền tố sẽ quá chậm. Điều này gợi ý rõ ràng rằng mỗi vị trí nên được xử lý tối đa một hoặc hai lần theo cách tham lam hoặc mang tính xây dựng. 

Một trường hợp phức tạp phát sinh khi các quyết định cục bộ ảnh hưởng đến các tiền tố trước đó. Ví dụ: một ý tưởng ngây thơ có thể là sửa từng tiền tố một cách độc lập nhưng điều đó sẽ phá vỡ các tiền tố trước đó. Một sai lầm khác là cố gắng đếm trên toàn cầu có bao nhiêu`a`Và`b`là cần thiết mà không tôn trọng ràng buộc từng tiền tố, bỏ qua thực tế là mỗi tiền tố áp đặt yêu cầu số dư riêng của nó dần dần. 

Ví dụ, nếu chúng ta có`bbbb`, một cái nhìn tổng thể ngây thơ nói rằng chúng ta cần hai`a`và hai`b`, nhưng không giải thích ngay nơi đặt chúng. Câu trả lời đúng phụ thuộc vào việc thực thi sự cân bằng ở mỗi bước tiền tố chẵn, không chỉ ở bước cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thực thi tiền tố điều kiện theo tiền tố. Đối với mỗi tiền tố có độ dài chẵn, chúng ta có thể kiểm tra xem nó có cân bằng hay không và nếu không, hãy thử tất cả các lần lật ký tự có thể có bên trong nó để khắc phục sự mất cân bằng. Điều này nhanh chóng trở thành cấp số nhân vì việc sửa một tiền tố có thể yêu cầu khám phá nhiều tổ hợp lần lật và mỗi tiền tố phụ thuộc vào các sửa đổi trước đó. Ngay cả việc tính toán lại tham lam cho mỗi tiền tố cũng sẽ tốn O(n) cho mỗi tiền tố, dẫn đến tổng thể là O(n²). 

Quan sát quan trọng là tiền tố không độc lập. Khi chúng ta di chuyển từ tiền tố có độ dài`i-2`đến tiền tố có độ dài`i`, chỉ có hai nhân vật mới được giới thiệu. Điều này có nghĩa là chúng ta có thể duy trì tình trạng mất cân bằng đang chạy cục bộ. 

Chúng ta có thể giải thích vấn đề này như việc xây dựng một chuỗi mục tiêu thỏa mãn một ràng buộc cấu trúc rất đơn giản: mỗi cặp`(1,2)`,`(3,4)`,`(5,6)`, v.v., phải tạo thành một`a`và một`b`theo một thứ tự nào đó. Mỗi cặp độc lập với tất cả các cặp khác vì sự cân bằng chỉ được kiểm tra ở các ranh giới chẵn chứ không phải ở các vị trí lẻ. Điều này tách vấn đề thành các quyết định độc lập cho mỗi cặp. 

Với mỗi cặp ta chọn cách sắp xếp (`ab`hoặc`ba`) giúp giảm thiểu việc lật so với các ký tự gốc. Tổng hợp những chi phí địa phương tối thiểu này sẽ mang lại kết quả tối ưu toàn cầu. 

Điều này có hiệu quả vì bất kỳ chuỗi cuối cùng hợp lệ nào cũng phải thỏa mãn ràng buộc rằng cứ hai vị trí liên tiếp đều chứa chính xác một`a`và một`b`và bất kỳ sai lệch nào bên trong một cặp chỉ ảnh hưởng đến giá của cặp đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi theo từng đoạn gồm hai ký tự. 

1. Chia chuỗi thành từng cặp`(s[0], s[1]), (s[2], s[3]), ...`. Mỗi cặp phải trở thành một`a`và một`b`. Điều này xuất phát trực tiếp từ yêu cầu rằng mọi tiền tố chẵn đều phải cân bằng. 
2. Với mỗi cặp, hãy tính chi phí để chuyển đổi nó thành`"ab"`, đó là số lượng không khớp giữa cặp hiện tại và`"ab"`. 
3. Đồng thời tính chi phí để chuyển đổi cùng một cặp thành`"ba"`. 
4. Chọn cái rẻ hơn trong hai phương án cho cặp đó. Nếu chi phí bằng nhau thì lựa chọn nào cũng hợp lệ. 
5. Áp dụng phép biến đổi đã chọn để xây dựng chuỗi cuối cùng. 
6. Tổng hợp tất cả các chi phí đã chọn để đạt được số lượng hoạt động tối thiểu. 

### Tại sao nó hoạt động 

Mỗi tiền tố chẵn kết thúc ở vị trí`2k`bao gồm chính xác đầu tiên`k`cặp. Điều kiện cân bằng cho tiền tố đó được thỏa mãn khi và chỉ khi mỗi cặp chứa đúng một`a`và một`b`. Không có sự tương tác giữa các cặp trong ràng buộc, vì vậy việc tối ưu hóa từng cặp một cách độc lập không thể gây hại cho các cặp khác. Điều này tạo ra sự phân rã vấn đề tối ưu hóa toàn cục thành các tối thiểu hóa cục bộ độc lập, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = list(input().strip())

ans = 0
res = []

for i in range(0, n, 2):
    a, b = s[i], s[i + 1]

    # cost to make "ab"
    cost_ab = (a != 'a') + (b != 'b')
    # cost to make "ba"
    cost_ba = (a != 'b') + (b != 'a')

    if cost_ab <= cost_ba:
        ans += cost_ab
        res.append('a')
        res.append('b')
    else:
        ans += cost_ba
        res.append('b')
        res.append('a')

print(ans)
print("".join(res))
```Giải pháp đọc chuỗi và xử lý hai ký tự cùng một lúc. Đối với mỗi cặp, nó đánh giá cả hai cấu hình hợp lệ có thể có và chọn cấu hình cần ít lần lật hơn. Chuỗi kết quả được xây dựng tăng dần, đảm bảo tính nhất quán với cấu trúc chi phí tối thiểu đã chọn. Bộ đếm câu trả lời theo dõi số lần lật được yêu cầu trên tất cả các cặp. 

Một điểm tinh tế phổ biến ở đây là đảm bảo rằng chúng tôi không bao giờ cố gắng thực thi sự cân bằng tiền tố trên toàn cầu. Tính đúng đắn hoàn toàn phụ thuộc vào việc phân tách thành các cặp độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
bbbb
```Chúng tôi xử lý các cặp:`(b,b)`Và`(b,b)`. 

| Cặp | Tùy chọn | Chi phí | Được chọn | 
| --- | --- | --- | --- | 
| bb | ab | 2 | | 
| bb | ba | 2 | hoặc | 
| bb | ab | 2 | | 
| bb | ba | 2 | hoặc | 

Chúng tôi chọn`ab`cho cặp đầu tiên và`ba`cho cặp thứ hai. 

Chuỗi cuối cùng trở thành`abba`, với tổng chi phí`2`. 

Điều này xác nhận rằng ngay cả khi tất cả các ký tự giống hệt nhau, việc ghép nối cục bộ vẫn tạo ra một cấu trúc cân bằng. 

### Ví dụ 2 

đầu vào:```
6
ababab
```Cặp:`(a,b)`,`(a,b)`,`(a,b)`. 

| Cặp | Tùy chọn | Chi phí | Được chọn | 
| --- | --- | --- | --- | 
| ab | ab | 0 | giữ | 
| ab | ba | 2 | | 
| ab | ab | 0 | giữ | 

Tất cả các cặp đã khớp`ab`, vì vậy không cần lật. Đầu ra vẫn không thay đổi. 

Điều này cho thấy thuật toán xác định chính xác các cấu trúc đã hợp lệ mà không cần sửa đổi không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cặp ký tự được xử lý một lần với công việc liên tục | 
| Không gian | O(n) | Chúng tôi xây dựng chuỗi kết quả | 

Độ phức tạp tuyến tính là đủ cho`n ≤ 2 × 10^5`và việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ chuỗi đầu ra, cũng là chuỗi tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input().strip())
    s = list(input().strip())

    ans = 0
    res = []

    for i in range(0, n, 2):
        a, b = s[i], s[i + 1]

        cost_ab = (a != 'a') + (b != 'b')
        cost_ba = (a != 'b') + (b != 'a')

        if cost_ab <= cost_ba:
            ans += cost_ab
            res.append('a')
            res.append('b')
        else:
            ans += cost_ba
            res.append('b')
            res.append('a')

    print(ans)
    print("".join(res))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\nbbbb\n") == "2\nabba"
assert run("4\nabba\n") == "0\nabba"

# custom cases
assert run("2\naa\n") == "1\nab"
assert run("2\nbb\n") == "1\nab"
assert run("2\nab\n") == "0\nab"
assert run("6\nbbbbbb\n") == "3\nababab"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 aa`|`1 ab`| hiệu chỉnh một cặp tối thiểu | 
|`2 bb`|`1 ab`| đối xứng với các ký tự đối diện | 
|`2 ab`|`0 ab`| cặp đã hợp lệ | 
|`6 bbbbbb`|`3 ababab`| cấu trúc trường hợp xấu nhất lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai ký tự trong một cặp giống hệt nhau, chẳng hạn như`"aa"`hoặc`"bb"`. Trong những tình huống này, cả hai cấu hình mục tiêu hợp lệ (`ab`Và`ba`) có cùng chi phí và việc chọn một trong hai không ảnh hưởng đến các cặp khác. Thuật toán xử lý chính xác điều này vì nó so sánh cả hai tùy chọn một cách độc lập trên mỗi cặp và cho phép các mối quan hệ. 

Đối với đầu vào:```
4
aaaa
```Xử lý: 

Cặp đầu tiên`"aa"`trở thành`"ab"`(chi phí 1), cặp thứ hai`"aa"`trở thành`"ab"`(chi phí 1). Tổng chi phí là 2. 

Dấu vết: 

| tôi | cặp | chi phí_ab | chi phí_ba | đã chọn | 
| --- | --- | --- | --- | --- | 
| 0 | aa | 1 | 1 | ab | 
| 2 | aa | 1 | 1 | ab | 

Đầu ra cuối cùng`abab`thỏa mãn cả hai ràng buộc tiền tố chẵn:`ab`được cân bằng và`abab`cũng được cân bằng. 

Điều này xác nhận rằng các quyết định tham lam cục bộ trên mỗi cặp là đủ ngay cả khi toàn bộ chuỗi là đồng nhất.
