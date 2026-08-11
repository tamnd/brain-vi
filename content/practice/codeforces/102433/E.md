---
title: "CF 102433E - Dây cầu vồng"
description: "Chúng tôi có một chuỗi chữ thường và chúng tôi muốn đếm các chuỗi con của nó có các ký tự được chọn đều khác nhau. Dãy con được xác định bởi các vị trí chúng ta chọn, do đó, việc chọn a đầu tiên từ aab và chọn a thứ hai là các dãy con khác nhau, mặc dù cả hai đều tạo ra…"
date: "2026-08-10T07:40:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 209
verified: true
draft: false
---

[CF 102433E - Dây cầu vồng](https://codeforces.com/problemset/problem/102433/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi chữ thường và chúng tôi muốn đếm các chuỗi con của nó có các ký tự được chọn đều khác nhau. Một dãy con được xác định bởi các vị trí chúng ta chọn, vì vậy việc chọn dãy đầu tiên`a`từ`aab`và chọn cái thứ hai`a`là các chuỗi con khác nhau, mặc dù cả hai đều tạo ra chuỗi một ký tự`a`. Dãy con trống cũng hợp lệ. 

Câu hỏi quan trọng không thực sự là về thứ tự của các chữ cái. Khi chúng tôi quyết định vị trí nào được chọn, thứ tự ban đầu của chúng sẽ tự động đưa ra một chuỗi tiếp theo. Đối với dãy cầu vồng, mỗi chữ cái có thể đóng góp 0 vị trí được chọn hoặc chính xác một vị trí được chọn. Sự quan sát đó biến vấn đề thành sản phẩm của những lựa chọn độc lập. 

Chuỗi có độ dài tối đa`100000`, do đó một thuật toán kiểm tra tất cả các dãy con là hoàn toàn không khả thi. có`2^n`các chuỗi con, vốn đã lớn về mặt thiên văn đối với`n = 100000`. Ngay cả một thuật toán với thời gian bậc hai, khoảng`10^10`hoạt động ở kích thước tối đa, sẽ vượt xa giới hạn một giây. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Vì`a`, câu trả lời đúng là`2`, bởi vì chúng ta có thể chọn`a`hoặc không chọn gì cả. Việc triển khai quên chuỗi con trống sẽ trả về`1`. 

Vì`aaa`, câu trả lời là`4`. Chúng ta có thể chọn không có vị trí nào trong ba vị trí hoặc chọn chính xác một trong ba vị trí. Việc chọn hai vị trí không hợp lệ vì dãy con kết quả chứa cùng một chữ cái hai lần. Một lỗi phổ biến là đếm các chuỗi kết quả khác nhau thay vì các lựa chọn vị trí khác nhau, điều này sẽ chỉ đưa ra kết quả không chính xác.`2`, cụ thể là chuỗi rỗng và`a`. 

Vì`aab`, câu trả lời là`6`. Các dãy con hợp lệ là dãy con trống, ba dãy con một ký tự và hai dãy con có độ dài hai ký tự hợp lệ chứa một`a`và`b`. Hai sự lựa chọn của`a`khác nhau vì họ sử dụng các vị trí khác nhau. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi tập hợp con của các vị trí. Đối với mỗi tập hợp con, chúng tôi có thể kiểm tra các ký tự đã chọn và kiểm tra xem có chữ cái nào xuất hiện hai lần hay không. Điều này đúng vì mỗi tập con của các vị trí biểu diễn chính xác một dãy con. Tuy nhiên, có`2^n`các tập hợp con và việc kiểm tra từng tập hợp con có thể mất tới`O(n)`thời gian. Độ phức tạp trong trường hợp xấu nhất là`O(n 2^n)`, với`2^100000`dãy con ứng cử viên ở kích thước đầu vào tối đa. Ngay cả việc tạo ra những ứng cử viên đó là không thể. 

Quan sát hữu ích là dãy con cầu vồng đặt ra một hạn chế cực kỳ đơn giản cho mỗi chữ cái một cách độc lập. Giả sử thư`c`xảy ra`cnt[c]`lần trong chuỗi gốc. Trong dãy con cầu vồng có chính xác`cnt[c] + 1`các khả năng cho bức thư này: chọn không xuất hiện lần nào hoặc chọn chính xác một trong các lần xuất hiện của nó`cnt[c]`lần xuất hiện. 

Những lựa chọn này là độc lập giữa các chữ cái khác nhau. Nếu chúng ta chọn một lần xuất hiện của`a`và một lần xuất hiện`b`, vị trí của chúng đã xác định được thứ tự trong dãy sau. Chúng tôi không cần phải thực hiện lựa chọn đặt hàng bổ sung. Do đó, mọi sự kết hợp của các lựa chọn trên mỗi chữ cái đều tạo ra chính xác một chuỗi con cầu vồng hợp lệ và mọi chuỗi con cầu vồng hợp lệ sẽ tương ứng với chính xác một chuỗi con như vậy. 

Với 26 chữ cái viết thường thì tổng số là 

[ 
\prod_{c='a'}^{'z'} (cnt[c] + 1). 
] 

Dãy con trống được bao gồm một cách tự nhiên bằng cách chọn số lần xuất hiện bằng 0 cho mỗi chữ cái. 

Lực lượng vũ phu hoạt động vì một chuỗi con được xác định hoàn toàn bởi các vị trí đã chọn của nó, nhưng nó không thành công vì có nhiều tập hợp con vị trí theo cấp số nhân. Quan sát cho thấy mỗi chữ cái có thể được chọn 0 hoặc một lần một cách độc lập sẽ giảm vấn đề xuống còn việc đếm 26 tần số và nhân 26 thừa số nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n 2^n)`|`O(n)`| Quá chậm | 
| Sản phẩm tần số |`O(n + 26)`|`O(26)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng tần số với một mục nhập cho mỗi chữ cái viết thường. Quét chuỗi một lần và tăng tần số tương ứng với mỗi ký tự. Điều này cho chúng ta số lượng vị trí có thể có mà từ đó mỗi chữ cái có thể được chọn. 
2. Khởi tạo câu trả lời cho`1`. Điều này thể hiện sự lựa chọn duy nhất trong đó không có chữ cái nào được chọn, đó là dãy con trống. 
3. Với mỗi chữ cái viết thường, hãy nhân kết quả với`cnt[c] + 1`và giảm nó theo modulo`11092019`. Hệ số đếm tất cả các khả năng xảy ra cho chữ cái đó: chọn không xuất hiện hoặc chọn chính xác một trong các lần xuất hiện của nó. 
4. In sản phẩm thu được. Vì sự lựa chọn của mỗi chữ cái là độc lập nên tích số đếm mọi dãy cầu vồng con chính xác một lần. 

### Tại sao nó hoạt động 

Xét bất kỳ dãy cầu vồng nào. Đối với mỗi chữ cái, ghi lại xem nó có vắng mặt hay không, nếu có thì sự xuất hiện của chữ cái đó đã được chọn. Vì dãy con không chứa các chữ cái lặp lại nên mô tả này có chính xác một lựa chọn cho mỗi chữ cái: không có lựa chọn nào nếu vắng mặt hoặc một trong các lần xuất hiện của nó nếu có. Ngược lại, bất kỳ tập hợp các lựa chọn cho mỗi chữ cái này đều chọn tối đa một vị trí cho mỗi chữ cái, do đó, các vị trí đã chọn luôn tạo thành một chuỗi con cầu vồng khi đọc theo thứ tự ban đầu của chúng. Do đó, có sự tương ứng một-một giữa các chuỗi cầu vồng và các tổ hợp được tính bằng 

[ 
\prod_c (cnt[c]+1). 
] 

Phép nhân đếm chính xác các đối tượng mong muốn, bao gồm cả dãy con trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 11092019

def solve():
    s = input().strip()

    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    ans = 1
    for x in cnt:
        ans = ans * (x + 1) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng mảng tần số. Vì đầu vào chỉ chứa các chữ cái tiếng Anh viết thường,`ord(ch) - ord('a')`ánh xạ mọi ký tự trực tiếp vào phạm vi`0`bởi vì`25`. 

Vòng lặp thứ hai áp dụng công thức tính tích. Bắt đầu với`ans = 1`là cần thiết vì dãy con trống phải được tính. Đối với một chữ cái xuất hiện`x`lần, nhân với`x + 1`tính đến mọi cách để bỏ qua chữ cái đó hoặc chọn một trong các lần xuất hiện của nó. 

Modulo được áp dụng sau mỗi lần nhân. Số nguyên Python không bị tràn, nhưng việc giữ giá trị giảm trong suốt quá trình tính toán là điều đương nhiên đối với câu trả lời được yêu cầu và giữ cho các giá trị trung gian ở mức nhỏ. Không có vấn đề riêng biệt nào trong hệ số tần số: nếu một chữ cái xuất hiện một lần thì hệ số của nó là`2`, đại diện cho "không chọn nó" và "chọn lần xuất hiện duy nhất của nó"; nếu nó xuất hiện 0 lần thì hệ số của nó là`1`và nó không có tác dụng. 

## Ví dụ đã hoạt động 

### Mẫu 1:`aab`Số đếm tần số là`a = 2`Và`b = 1`, trong khi mọi chữ cái khác đều có tần số bằng 0. Sản phẩm do đó trở thành`(2 + 1)(1 + 1) = 6`. 

| Nhân vật | Tần số | Yếu tố | Chạy câu trả lời | 
| --- | --- | --- | --- | 
|`a`| 2 | 3 | 3 | 
|`b`| 1 | 2 | 6 | 
|`c`bởi vì`z`| 0 | mỗi cái 1 | 6 | 

Ba sự lựa chọn cho`a`là chọn không xảy ra lần nào, chọn lần xuất hiện đầu tiên hoặc chọn lần xuất hiện thứ hai. Hai sự lựa chọn cho`b`có nên chọn nó hay không. Sự kết hợp của chúng cho sáu dãy con hợp lệ, bao gồm cả dãy trống. 

### Mẫu 2:`icpcprogrammingcontest`Các tần số khác 0 là`c = 3`, tám chữ cái với tần số`2`, cụ thể là`g`,`i`,`m`,`n`,`o`,`p`,`r`, Và`t`, và ba chữ cái có tần số`1`, cụ thể là`a`,`e`, Và`s`. 

| Nhóm tần số | Số chữ cái | Yếu tố mỗi chữ cái | Đóng góp | 
| --- | --- | --- | --- | 
| 3 | 1 | 4 | 4 | 
| 2 | 8 | 3 | 6561 | 
| 1 | 3 | 2 | 8 | 
| 0 | 14 | 1 | 1 | 

Câu trả lời cuối cùng là 

[ 
4 \times 3^8 \times 2^3 = 209952. 
] 

Dấu vết cho thấy tại sao các vị trí thực tế không cần phải được xem xét sau khi đếm tần số. Mỗi lần chúng ta chọn một lần xuất hiện từ mỗi chữ cái đã chọn, các chỉ mục ban đầu sẽ tự động thiết lập thứ tự tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + 26)`| Một lần đếm các ký tự, sau đó 26 yếu tố được nhân lên. | 
| Không gian |`O(26)`| Chỉ tần số của mỗi chữ cái viết thường được lưu trữ. | 

Với`n`lên đến`100000`, thuật toán chỉ thực hiện một lần quét tuyến tính của đầu vào, sau đó là 26 thao tác có kích thước không đổi. Điều này dễ dàng nằm trong giới hạn dự định, không giống như bất kỳ cách tiếp cận nào liệt kê hoặc so sánh các chuỗi tiếp theo. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 11092019

def solution(s):
    cnt = [0] * 26

    for ch in s:
        cnt[ord(ch) - ord('a')] += 1

    ans = 1
    for x in cnt:
        ans = ans * (x + 1) % MOD

    return str(ans)

def run(inp: str) -> str:
    return solution(inp.strip())

# Provided samples
assert run("aab") == "6", "sample 1"
assert run("icpcprogrammingcontest") == "209952", "sample 2"

# Minimum-size input
assert run("a") == "2", "single character"

# All characters equal
assert run("aaa") == "4", "three identical characters"

# Every character distinct
assert run("abcdefghijklmnopqrstuvwxyz") == str(pow(2, 26, MOD)), \
    "all 26 letters occur once"

# Maximum-size input
assert run("a" * 100000) == "100001", \
    "100000 identical characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`2`| Đầu vào tối thiểu và bao gồm dãy con trống | 
|`aaa`|`4`| Các chữ cái lặp đi lặp lại và các chuỗi dựa trên vị trí | 
|`abcdefghijklmnopqrstuvwxyz`|`67108864`| Tất cả 26 chữ cái đều xuất hiện một lần nên mọi tập hợp con đều là cầu vồng | 
|`"a" * 100000`|`100001`| Kích thước đầu vào tối đa và trường hợp ranh giới chữ cái lặp lại | 

Đối với bài kiểm tra phân biệt hoàn toàn, mỗi chữ cái trong số 26 chữ cái đều có thể được chọn hoặc bỏ qua một cách độc lập, vì vậy câu trả lời là`2^26 = 67108864`. Đối với tất cả kích thước tối đa-`a`kiểm tra, chỉ có 0 hoặc một trong các`100000`các vị trí có thể được lựa chọn, đưa ra`100001`. 

## Vỏ cạnh 

Đối với đầu vào một ký tự`a`, tần số của`a`là`1`, vậy hệ số của nó là`2`. Mỗi nhân vật khác đều đóng góp một yếu tố`1`. Câu trả lời cuối cùng là`2`, tương ứng với dãy con trống và dãy con chứa vị trí duy nhất. 

Vì`aaa`, tần số của`a`là`3`, vậy câu trả lời là`3 + 1 = 4`. Bốn lựa chọn là chọn không`a`, chọn vị trí một, chọn vị trí hai hoặc chọn vị trí ba. Thuật toán không bao giờ tính một cặp vị trí vì mỗi chữ cái chỉ có một ô lựa chọn trong dãy con cầu vồng. 

Vì`aab`, các yếu tố là`3`vì`a`Và`2`vì`b`, sản xuất`6`. Điều này mắc phải sai lầm khi đếm các chuỗi kết quả riêng biệt thay vì các lựa chọn vị trí riêng biệt. Hai dãy con chứa`a`Và`b`có thể sử dụng một trong hai`a`vị trí, vì vậy cả hai đều phải được tính. 

Đối với một chuỗi chứa mỗi chữ cái viết thường đúng một lần, mọi dãy con sẽ tự động có dạng cầu vồng. có`2^26`tập hợp con của các vị trí và công thức tích cho kết quả chính xác`2`cho mỗi chữ cái trong số 26 chữ cái, tạo ra`2^26`. Điều này kiểm tra xem thuật toán có xử lý chính xác số lượng chữ cái riêng biệt tối đa có thể không. 

Vì`100000`bản sao của`a`, câu trả lời là`100001`. Chỉ có thể chọn một vị trí vì việc chọn hai vị trí sẽ lặp lại`a`. Thuật toán có được điều này trực tiếp từ yếu tố duy nhất`100000 + 1`, chứng tỏ rằng tần số lớn không yêu cầu lập trình động trên các vị trí.
