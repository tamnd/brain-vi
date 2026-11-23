---
title: "CF 464C - Thay thế về số lượng"
description: "Chúng ta bắt đầu bằng một chuỗi thập phân s. Sau đó, một chuỗi các quy tắc thay thế được áp dụng theo thứ tự. Mỗi quy tắc có dạng d - t, nghĩa là mỗi lần xuất hiện của chữ số d đều được thay thế bằng chuỗi t."
date: "2026-06-07T17:11:27+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 464
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 265 (Div. 1)"
rating: 2100
weight: 464
solve_time_s: 75
verified: true
draft: false
---

[CF 464C - Số thay thế](https://codeforces.com/problemset/problem/464/C) 

**Xếp hạng:** 2100 
**Thẻ:** dp 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu bằng một chuỗi thập phân`s`. Sau đó, một chuỗi các quy tắc thay thế được áp dụng theo thứ tự. Mỗi quy tắc có dạng`d -> t`, nghĩa là mỗi lần xuất hiện của chữ số`d`được thay thế bằng chuỗi`t`. 

Một chi tiết quan trọng là việc thay thế không được thực hiện trên chuỗi hiện tại từng ký tự một. Mỗi quy tắc biến đổi toàn bộ chuỗi hiện tại. Vì các quy tắc sau này có thể xác định lại các chữ số xuất hiện bên trong các chuỗi thay thế trước đó, nên việc khai triển cuối cùng của một chữ số phụ thuộc vào tất cả các quy tắc theo sau nó. 

Sau khi áp dụng tất cả các thay thế, chuỗi kết quả được hiểu là số thập phân và chúng ta phải tính giá trị của nó theo modulo`10^9 + 7`. Các số 0 đứng đầu vẫn là một phần của biểu diễn chuỗi nhưng chúng không ảnh hưởng đến giá trị số. 

Các ràng buộc ngay lập tức loại trừ mọi mô phỏng của chuỗi cuối cùng. Chuỗi ban đầu có thể có độ dài lên tới`10^5`, và có thể có tới`10^5`quy tắc thay thế. Mặc dù tổng chiều dài của tất cả các cạnh bên phải chỉ bằng`10^5`, sự thay thế lặp đi lặp lại có thể làm cho chuỗi cuối cùng về mặt khái niệm có kích thước lớn về mặt thiên văn. Một chữ số có thể mở rộng thành một chuỗi dài, các chữ số của chuỗi này lại mở rộng, v.v. Kết quả thực tế có thể dễ dàng có nhiều ký tự theo cấp số nhân. 

Điều này có nghĩa là chúng ta không bao giờ được xây dựng chuỗi cuối cùng. Chúng ta cần một biểu diễn nén cho phép chúng ta tính toán số kết quả theo modulo`10^9 + 7`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
s = "3"
rule: 3->
```Chuỗi cuối cùng trống. Vấn đề xác định số kết quả là`0`. Bất kỳ cách tiếp cận nào giả định mọi thay thế đều tạo ra ít nhất một chữ số sẽ thất bại ở đây. 

Coi như:```
s = "2"
rules:
2->00
```Chuỗi cuối cùng là`"00"`. Về mặt số lượng thì đây vẫn là`0`. Thuật toán phải hoạt động với độ dài chuỗi và giá trị mô-đun riêng biệt thay vì cố gắng loại bỏ các số 0 ở đầu. 

Coi như:```
s = "1"
rules:
1->23
2->4
```Kết quả cuối cùng đúng là`"43"`, không`"23"`. Quy tắc thứ hai ảnh hưởng đến chữ số`2`được giới thiệu bởi quy tắc đầu tiên. Sự phụ thuộc giữa các quy tắc này là khó khăn trung tâm của vấn đề. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Duy trì chuỗi hiện tại và áp dụng từng thay thế theo nghĩa đen. Đối với mỗi ký tự bằng chữ số đích, hãy nối thêm chuỗi thay thế. 

Điều này đúng vì nó tuân theo chính xác phát biểu vấn đề. Vấn đề là tăng trưởng kích thước. Ngay cả một chuỗi nhỏ như```
1->11
```lặp đi lặp lại nhiều lần tăng gấp đôi chiều dài ở mỗi bước. Chuỗi cuối cùng có thể trở nên lớn theo cấp số nhân, vượt xa giới hạn bộ nhớ. 

Quan sát quan trọng là chúng ta không cần chính chuỗi cuối cùng. Để tính một số thập phân modulo`M`, một chuỗi chỉ đóng góp hai phần thông tin: 

1. Its length.
 2. Giá trị số của nó theo modulo`M`. 

Giả sử một chuỗi`A`có giá trị`val(A)`và chiều dài`len(A)`, và một chuỗi`B`có giá trị`val(B)`và chiều dài`len(B)`. 

Để ghép nối`AB`:$$val(AB)=val(A)\cdot 10^{len(B)}+val(B)$$modulo`M`. 

Điều này có nghĩa là mọi chữ số mở rộng có thể được biểu diễn bằng một cặp:$$(value,\ length)$$mà không bao giờ lưu trữ chuỗi thực tế. 

Bây giờ hãy xem các quy tắc thay thế. Ý nghĩa của một chữ số phụ thuộc vào các quy tắc trong tương lai chứ không phải trong quá khứ. Điều đó gợi ý việc xử lý ngược các quy tắc. 

Hãy tưởng tượng chúng ta đã biết khai triển cuối cùng của mỗi chữ số sau khi áp dụng tất cả các quy tắc sau này. Sau đó cho một quy tắc```
d -> t
```chúng ta có thể tính toán khai triển cuối cùng của`d`bằng cách nối các phần mở rộng đã biết của các chữ số bên trong`t`. 

Việc xử lý từ quy tắc cuối cùng đến quy tắc đầu tiên sẽ đưa ra chính xác tình huống này. 

Ban đầu, trước khi bất kỳ quy tắc nào được xử lý, mỗi chữ số sẽ mở rộng thành chính nó. Sau đó, mỗi bước lùi sẽ cập nhật một chữ số bằng cách sử dụng cách mở rộng hiện tại của các chữ số xuất hiện ở phía bên phải. 

Sau khi tất cả các quy tắc đã được xử lý, chúng ta biết phần mở rộng cuối cùng của mỗi chữ số trong chuỗi gốc. Bản quét cuối cùng của`s`tính toán câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | Hàm mũ | Quá chậm | 
| Tối ưu | O( | s | + tổng chiều dài thay thế) | 

## Hướng dẫn thuật toán 

1. Hãy để`MOD = 10^9 + 7`. 
2. Tính toán trước lũy thừa của mười modulo`MOD`lên đến mức đóng góp chiều dài mở rộng tối đa có thể. Vì tổng chiều dài của tất cả các chuỗi thay thế tối đa là`10^5`, sức mạnh tính toán lên đến phạm vi đó là đủ. 
3. Với mỗi chữ số`0...9`, duy trì:`val[d]`= giá trị modulo mở rộng cuối cùng của nó`MOD`.`len[d]`= chiều dài modulo mở rộng cuối cùng của nó`MOD-1`. 
4. Khởi tạo mỗi chữ số để thể hiện chính nó:`val[d] = d`

`len[d] = 1`5. Đọc tất cả các quy tắc và lưu trữ chúng. 
6. Xử lý các quy tắc từ đầu đến cuối. 
7. Vì một quy tắc`d -> t`, xây dựng phần mở rộng của`d`bằng cách quét`t`từ trái sang phải. 
8. Duy trì hai biến:`cur_val`

`cur_len`Ban đầu cả hai đều bằng không. 
9. Với mỗi chữ số`x`TRONG`t`, nối phần mở rộng đã được tính toán của`x`:$$cur\_val = cur\_val \cdot 10^{len[x]} + val[x] \pmod{MOD}$$

$$cur\_len = cur\_len + len[x] \pmod{MOD-1}$$1. Sau khi xử lý xong toàn bộ phía bên phải, gán:$$val[d] = cur\_val$$

$$len[d] = cur\_len$$1. Sau khi tất cả các quy tắc đã được xử lý, hãy xử lý chuỗi gốc`s`. 
2. Bắt đầu từ số 0, nối khai triển cuối cùng của mỗi chữ số trong`s`sử dụng cùng một công thức. 
3. Xuất giá trị kết quả theo modulo`MOD`. 

Lý do độ dài được lưu trữ modulo`MOD-1`là lũy thừa của mười luôn được lấy theo modulo`MOD`, đó là số nguyên tố. Theo định lý Fermat:$$10^k \bmod MOD = 10^{k \bmod (MOD-1)} \bmod MOD$$bởi vì`10`Và`MOD`là nguyên tố cùng nhau. 

### Tại sao nó hoạt động 

Xử lý các quy tắc theo thứ tự ngược lại. Sau khi xử lý tất cả các quy tắc từ vị trí`i+1`trở đi, cặp`(val[d], len[d])`mô tả chính xác chuỗi thu được từ chữ số`d`sau khi áp dụng những quy tắc sau này. 

Khi xử lý quy tắc`i`, mọi chữ số xuất hiện ở phía bên phải của nó đã có biểu diễn cuối cùng dưới tất cả các phép thay thế sau này. Việc ghép các biểu diễn đó mang lại chính xác biểu diễn cuối cùng của chữ số bên trái sau quy tắc`i`và tất cả các quy tắc tiếp theo. 

Bằng cách quy nạp theo thứ tự xử lý ngược, mọi cặp được lưu trữ vẫn đúng. Sau khi quy tắc đầu tiên được kết hợp, cặp chữ số bằng với phần mở rộng cuối cùng hoàn chỉnh của nó. Phép nối cuối cùng trên chuỗi gốc sẽ xây dựng lại giá trị của số được chuyển đổi hoàn toàn, modulo`10^9 + 7`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    s = input().strip()
    n = int(input())

    rules = []
    for _ in range(n):
        line = input().strip()
        d = int(line[0])
        t = line[3:]
        rules.append((d, t))

    max_len = 100000
    pow10 = [1] * (max_len + 1)
    for i in range(1, max_len + 1):
        pow10[i] = (pow10[i - 1] * 10) % MOD

    val = [i for i in range(10)]
    length = [1] * 10

    for d, t in reversed(rules):
        cur_val = 0
        cur_len = 0

        for ch in t:
            x = ord(ch) - ord('0')
            cur_val = (cur_val * pow10[length[x]] + val[x]) % MOD
            cur_len = (cur_len + length[x]) % (MOD - 1)

        val[d] = cur_val
        length[d] = cur_len

    ans = 0
    for ch in s:
        x = ord(ch) - ord('0')
        ans = (ans * pow10[length[x]] + val[x]) % MOD

    print(ans)

solve()
```Giải pháp lưu trữ hai đại lượng cho mỗi chữ số.`val[d]`là giá trị thập phân của modulo khai triển đầy đủ của chữ số`10^9+7`.`length[d]`là modulo chiều dài mở rộng`10^9+6`. 

Thứ tự xử lý ngược lại là ý tưởng cốt lõi. Khi một quy tắc được truy cập, mọi chữ số xuất hiện ở phía bên phải của nó đều đã biết phần mở rộng cuối cùng của nó
