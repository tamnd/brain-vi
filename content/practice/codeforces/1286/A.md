---
title: "CF 1286A - Vòng Hoa"
description: "Chúng ta có một hàng n bóng đèn, mỗi bóng được đánh số từ 1 đến n. Một số bóng đèn bị thiếu, được biểu thị bằng số 0 trong mảng đầu vào."
date: "2026-06-11T19:09:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1286
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 612 (Div. 1)"
rating: 1800
weight: 1286
solve_time_s: 162
verified: true
draft: false
---

[CF 1286A - Garland](https://codeforces.com/problemset/problem/1286/A) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, sắp xếp 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng`n`bóng đèn, mỗi bóng đèn được đánh số từ`1`ĐẾN`n`. Một số bóng đèn bị thiếu, được biểu thị bằng số 0 trong mảng đầu vào. Mục tiêu của chúng tôi là điền vào các bóng đèn còn thiếu bằng cách sử dụng các số chưa có và đặt chúng theo trình tự sao cho giảm thiểu số lượng các cặp liền kề có số chẵn lẻ khác nhau. Độ phức tạp trong bài toán này là đếm xem có bao nhiêu cặp liền kề có một số chẵn và một số lẻ. Ví dụ, nếu một chuỗi là`1 4 2 3 5`, có hai sự chuyển tiếp giữa số lẻ và số chẵn:`(1,4)`Và`(2,3)`. 

Kích thước đầu vào nhỏ:`n`có thể lên tới 100. Điều này có nghĩa là chúng ta có đủ khả năng chi trả`O(n^3)`hoặc thậm chí`O(n^4)`cách tiếp cận, nhưng một`O(n^2)`giải pháp lập trình động sạch hơn và mạnh mẽ hơn. Một cách tiếp cận bất cẩn chỉ tham lam đặt những số còn thiếu nhỏ nhất mà không xem xét tính chẵn lẻ có thể dễ dàng tạo ra những chuyển đổi không cần thiết. Ví dụ, nếu`n=4`và trình tự là`0 0 1 0`, việc điền các số 0 từ trái sang phải mà không nghĩ đến tính chẵn lẻ có thể mang lại kết quả`[2,3,1,4]`, tạo ra ba chuyển tiếp chẵn lẻ, trong khi`[3,2,1,4]`chỉ mang lại một. 

Trường hợp cạnh xảy ra khi tất cả các vị trí bị thiếu đều liên tiếp hoặc ở cuối. Trong những trường hợp này, chúng ta cần theo dõi tính chẵn lẻ của bóng đèn được đặt cuối cùng để tránh sự phức tạp thêm. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu ngây thơ sẽ thử mọi hoán vị có thể có của các số bị thiếu, đếm độ phức tạp kết quả cho mỗi số và trả về mức tối thiểu. Điều này hoạt động vì có`k`thiếu số nên về nguyên tắc cần phải có`k!`sắp xếp. Tuy nhiên, trong trường hợp xấu nhất`k=n=100`, lớn về mặt thiên văn (`100!`) và rõ ràng là không thể thực hiện được. Ngay cả việc thử mọi cách để phân chia các số còn thiếu thành các nhóm lẻ và chẵn cũng nhanh chóng bùng nổ trong các tổ hợp. 

Quan sát quan trọng là độ phức tạp chỉ phụ thuộc vào tính chẵn lẻ của các số liền kề chứ không phụ thuộc vào giá trị chính xác của chúng. Điều này cho phép chúng ta rút gọn vấn đề thành **vấn đề lập trình động trên các vị trí và số lượng số lẻ và số chẵn còn lại**. Cụ thể, đối với mỗi vị trí trong mảng, chúng ta có thể theo dõi độ phức tạp tối thiểu nếu số trước đó là số lẻ hoặc số chẵn và số bóng đèn chẵn và lẻ chưa sử dụng còn lại. Trạng thái DP là`(pos, odd_remaining, even_remaining, prev_parity)`và quá trình chuyển đổi đặt một số lẻ hoặc số chẵn ở vị trí 0 hiện tại, cập nhật độ phức tạp nếu nó khác với`prev_parity`. 

Cấu trúc này hoạt động vì mọi quyết định chỉ phụ thuộc vào tính chẵn lẻ của số trước đó và số lượng bóng đèn của mỗi loại còn lại. Chúng ta không bao giờ cần biết giá trị chính xác. Chúng tôi sử dụng tính năng ghi nhớ để lưu trữ các kết quả trung gian và tránh tính toán lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k!) | O(n) | Quá chậm | 
| DP trên tính chẵn lẻ | O(n^3) | O(n^3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định những số còn thiếu trong mảng và phân loại chúng thành số lẻ và số chẵn. Nếu tổng số là từ`1`ĐẾN`n`, tính những số còn thiếu bằng cách trừ đi những số đã có. 
2. Xác định bảng DP`dp[pos][odd_left][even_left][prev_parity]`, Ở đâu`pos`là chỉ số hiện tại,`odd_left`Và`even_left`đếm xem còn lại bao nhiêu bóng đèn lẻ và bóng đèn chẵn chưa sử dụng, và`prev_parity`là độ chẵn lẻ của bóng đèn được đặt trước đó. Khởi tạo tất cả các giá trị thành vô cùng, ngoại trừ vị trí bắt đầu không đặt bóng đèn và`prev_parity`có thể không được xác định. 
3. Lặp lại các vị trí. Nếu bóng đèn đã được đặt (khác 0), hãy cập nhật DP bằng cách chỉ xem xét các chuyển đổi phù hợp với tính chẵn lẻ của nó, tăng độ phức tạp nếu tính chẵn lẻ của nó khác với`prev_parity`. Nếu bóng đèn bị thiếu (không), hãy thử đặt một bóng đèn lẻ hoặc chẵn nếu còn lại. Cập nhật bảng DP bằng cách lấy độ phức tạp tối thiểu cho cả hai lựa chọn. 
4. Cuối cùng, câu trả lời là giá trị nhỏ nhất trong số`dp[n][0][0][0]`Và`dp[n][0][0][1]`, đại diện cho tất cả các bóng đèn được đặt với bóng đèn cuối cùng là chẵn hoặc lẻ. 
5. Trả lại mức tối thiểu này là độ phức tạp tối thiểu của vòng hoa. 

Tại sao nó hoạt động: DP đảm bảo tính chính xác vì ở mỗi bước chúng tôi xem xét tất cả các lựa chọn hợp lệ về tính chẵn lẻ cho các số 0 và theo dõi chính xác số bóng đèn lẻ/chẵn còn lại. Vì độ phức tạp chỉ phụ thuộc vào chuyển đổi chẵn lẻ nên trạng thái DP nắm bắt đầy đủ thông tin cần thiết cho các quyết định tối ưu. Việc ghi nhớ đảm bảo rằng các bài toán con chồng chéo chỉ được tính một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))

used = [False] * (n + 1)
for x in p:
    if x != 0:
        used[x] = True

odd_left = sum(1 for i in range(1, n+1) if not used[i] and i % 2 == 1)
even_left = sum(1 for i in range(1, n+1) if not used[i] and i % 2 == 0)

INF = 10**9
dp = [[[INF, INF] for _ in range(even_left+1)] for _ in range(odd_left+1)]
dp[odd_left][even_left][0] = 0
dp[odd_left][even_left][1] = 0

for idx, val in enumerate(p):
    new_dp = [[[INF, INF] for _ in range(even_left+1)] for _ in range(odd_left+1)]
    for o in range(odd_left+1):
        for e in range(even_left+1):
            for prev_parity in [0,1]:
                if dp[o][e][prev_parity] >= INF:
                    continue
                if val != 0:
                    cur_parity = val % 2
                    add = 0 if idx == 0 or cur_parity == prev_parity else 1
                    new_dp[o][e][cur_parity] = min(new_dp[o][e][cur_parity], dp[o][e][prev_parity] + add)
                else:
                    if o > 0:
                        cur_parity = 1
                        add = 0 if idx == 0 or cur_parity == prev_parity else 1
                        new_dp[o-1][e][cur_parity] = min(new_dp[o-1][e][cur_parity], dp[o][e][prev_parity] + add)
                    if e > 0:
                        cur_parity = 0
                        add = 0 if idx == 0 or cur_parity == prev_parity else 1
                        new_dp[o][e-1][cur_parity] = min(new_dp[o][e-1][cur_parity], dp[o][e][prev_parity] + add)
    dp = new_dp

res = min(dp[0][0][0], dp[0][0][1])
print(res)
```Giải thích: Đầu tiên chúng ta đếm các số lẻ và số chẵn chưa sử dụng. Sau đó, chúng tôi sử dụng DP`(odd_left, even_left, prev_parity)`thay vì chỉ số đầy đủ về hiệu quả. Đối với mỗi vị trí, chúng tôi truyền bá trạng thái trước đó sang trạng thái tiếp theo, tăng độ phức tạp nếu tính chẵn lẻ thay đổi. Chúng tôi kiểm tra cẩn thận các ranh giới để tránh các chỉ số tiêu cực. Độ phức tạp tối thiểu được thực hiện sau khi xử lý tất cả các vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5\n0 5 0 2 3\n`| idx | giá trị | o | e | trước_chẵn lẻ | new_dp[lẻ][chẵn][cur_chẵn lẻ] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | - | Hãy thử lẻ:1, chẵn:2 | 
| 1 | 5 | - | - | - | Chỉ xem xét chẵn lẻ=1, thêm 0 nếu có cùng chẵn lẻ, nếu không thì 1 | 
| 2 | 0 | 1 | 1 | - | Hãy thử vị trí chẵn/lẻ | 
| 3 | 2 | - | - | - | Chỉ xem xét tính chẵn lẻ=0 | 
| 4 | 3 | - | - | - | Chỉ xem xét tính chẵn lẻ=1 | 

Kết quả:`2`chuyển tiếp. 

### Ví dụ 2 

đầu vào:`7\n0 7 3 5 0 0 0\n`Việc xử lý tất cả các số 0 trong khi theo dõi các bóng đèn lẻ/chẵn còn lại sẽ dẫn đến một quá trình chuyển đổi duy nhất ở vị trí tối ưu. 

Những dấu vết này xác nhận rằng DP của chúng tôi xử lý chính xác cả số 0 bên trong và số 0 đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau: Odd_left (<n/2), Even_left (<n/2), prev_parity (2) | 
| Không gian | O(n^3) | Bảng DP có kích thước (odd_left+1) * (even_left+1) * 2 | 

Được cho`n≤100`, các hoạt động trong trường hợp xấu nhất theo thứ tự 50^2_2_100=~500.000, có thể chấp nhận được trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open('solution.py').read())  # assuming code above is saved as solution.py
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n0 5 0
```
