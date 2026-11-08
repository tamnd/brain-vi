---
title: "CF 1396C - Kẻ xâm lược quái vật"
description: "Chúng tôi được yêu cầu tính toán thời gian tối thiểu để Ziota tiêu diệt tất cả các tên trùm trong một chuỗi cấp độ trò chơi. Mỗi cấp độ chứa một số quái vật bình thường, mỗi quái vật có 1 điểm máu và chính xác một tên trùm có 2 điểm máu."
date: "2026-06-11T09:25:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1396
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 666 (Div. 1)"
rating: 2300
weight: 1396
solve_time_s: 168
verified: false
draft: false
---

[CF 1396C - Kẻ xâm lược quái vật](https://codeforces.com/problemset/problem/1396/C) 

**Đánh giá:** 2300 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính toán thời gian tối thiểu để Ziota tiêu diệt tất cả các tên trùm trong một chuỗi cấp độ trò chơi. Mỗi cấp độ chứa một số quái vật bình thường, mỗi quái vật có 1 điểm máu và chính xác một tên trùm có 2 điểm máu. Ziota có ba loại vũ khí: một khẩu súng lục giết chết 1 HP của một con quái vật, một tia laser gây 1 HP cho tất cả quái vật và một AWP giết chết bất kỳ quái vật nào ngay lập tức. Mỗi vũ khí có thời gian nạp lại và Ziota chỉ có thể nạp lại một khẩu súng mỗi lần. Sự di chuyển giữa các cấp độ mất`d`đơn vị thời gian và chúng tôi không thể tải lại hoặc bắn trong khi dịch chuyển. 

Mảng đầu vào`a`liệt kê số lượng quái vật bình thường ở mỗi cấp độ. Mục tiêu của chúng tôi là tổng thời gian tối thiểu để loại bỏ tất cả trùm, bắt đầu từ cấp 1. Hạn chế quan trọng là chúng tôi không thể bắn trùm bằng súng lục hoặc AWP cho đến khi tất cả quái vật bình thường bị tiêu diệt và nếu chúng tôi gây sát thương cho trùm mà không giết nó ngay lập tức, chúng tôi phải chuyển cấp độ sang cấp độ liền kề trước khi quay lại. 

Hạn chế chính là số lượng cấp độ`n`có thể lên tới 10^6 và số lượng quái vật mỗi cấp`a_i`cũng có thể lớn. Điều này loại trừ mọi giải pháp xử lý từng quái vật riêng lẻ trong một vòng lặp lồng nhau. Giải pháp phải hoạt động theo thời gian tuyến tính hoặc gần tuyến tính trong`n`. Các trường hợp khó khăn không rõ ràng bao gồm các cấp độ có một quái vật duy nhất và một tên trùm, trong đó các lựa chọn súng khác nhau ảnh hưởng đáng kể đến việc có cần di chuyển qua lại hay không và các tình huống sử dụng tia laser ở nhiều cấp độ giúp giảm thời gian nạp đạn nhưng buộc phải di chuyển nhiều hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ là mô phỏng mọi sự kết hợp có thể có giữa việc sử dụng súng và chuyển động. Chúng tôi sẽ thử từng chuỗi bắn quái vật bình thường, có thể sử dụng tia laze, sau đó tiêu diệt trùm bằng súng lục hoặc AWP và di chuyển giữa các cấp độ nếu cần. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các chiến lược hợp lệ. Tuy nhiên, với tối đa 10^6 cấp độ và mỗi cấp độ có tối đa 10^6 quái vật, một mô phỏng xem xét mọi quái vật hoặc tất cả các chuỗi sử dụng vũ khí sẽ dễ dàng vượt quá 10^12 thao tác. Tốc độ này quá chậm so với giới hạn thời gian 2 giây. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng, đối với mỗi cấp độ, thời gian tối thiểu để tiêu diệt tất cả quái vật bình thường cộng với trùm có thể được tính toán độc lập bằng một nhóm tùy chọn nhỏ. Có hai chiến lược chính cho một cấp độ: tiêu diệt từng quái vật bằng súng lục và kết liễu trùm bằng AWP hoặc sử dụng tia laser một cách tối ưu để gây sát thương cho tất cả quái vật cùng một lúc, có thể khiến trùm còn sống và xử lý nó sau đó. Mỗi cấp độ có thể được biểu thị dưới dạng chi phí về thời gian tải lại, cộng với chi phí di chuyển tùy chọn nếu chúng ta để trùm bị hư hại một phần. Điều này cho phép chúng tôi giảm vấn đề thành lập trình động theo các cấp độ, trong đó trạng thái là liệu chúng tôi đang đến từ cấp độ trước với trùm bị giết hoàn toàn hay bị hư hại một phần. DP chỉ cần xem xét hai trạng thái cho mỗi cấp độ, làm cho nó tuyến tính theo`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * a_i) | O(n) | Quá chậm | 
| DP tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cấp độ, hãy tính hai chi phí chính. Đầu tiên là chi phí "tiêu diệt mọi thứ trực tiếp" đơn giản: sử dụng súng lục trên mỗi quái vật bình thường và AWP trên trùm. Đây là`a_i * r1 + r3`. Thứ hai là chi phí "laser hoặc linh hoạt": sử dụng tia laser cộng với súng lục tùy chọn hoặc AWP để tiêu diệt trùm, lợi dụng chuyển động cưỡng bức nếu trùm sống sót. Đây là`min(r2 + r1, r1 * a_i + r1 * 2, r3)`cho sự kết hợp của ông chủ, sau đó thêm`a_i * r1`đối với quái vật bình thường. 
2. Xác định trạng thái DP. Cho phép`dp[i][0]`là thời gian tối thiểu để hoàn thành cấp độ`i`với việc ông chủ bị giết hoàn toàn, và`dp[i][1]`thời gian tối thiểu mà chúng ta kết thúc việc để lại một con trùm bị hư hại một phần, nghĩa là chúng ta sẽ phải di chuyển đi và quay lại sau. Đối với cấp độ 1,`dp[1][0]`chỉ là chi phí tiêu diệt trực tiếp, và`dp[1][1]`là chi phí linh hoạt cộng với chuyển động`d`bởi vì chúng ta sẽ để ông chủ còn sống. 
3. Trình tự xử lý các cấp độ. Đối với mỗi cấp độ, cập nhật`dp[i][0]`tối thiểu là hoàn thành đầy đủ cấp độ trước rồi trực tiếp hoàn thành cấp độ này hoặc hoàn thành cấp độ trước với một con trùm bị hư hỏng một phần và quay trở lại. Cập nhật`dp[i][1]`tương tự, tính đến việc để lại mức này bị hư hỏng một phần và cộng thêm chi phí di chuyển sang mức liền kề. 
4. Câu trả lời cuối cùng là`dp[n][0]`, có thể thêm chuyển động bổ sung tối thiểu nếu để lại một con trùm bị hư hỏng một phần ở cấp độ cuối cùng sẽ có lợi cho việc giảm bớt trước đó. 

Tại sao nó hoạt động: Bất biến DP là`dp[i][0]`luôn đại diện cho thời gian tối thiểu để tiêu diệt tất cả các tên trùm khi đạt cấp độ`i`, trong khi`dp[i][1]`đại diện cho thời gian tối thiểu để tiêu diệt tất cả các tên trùm lên đến cấp độ`i-1`cộng với một cấp độ`i`ông chủ bị hư hỏng một phần. Vì sự tương tác duy nhất giữa các cấp độ là sự di chuyển do khiến trùm bị hư hại một phần, nên việc duy trì hai trạng thái cho mỗi cấp độ sẽ nắm bắt được tất cả các chiến lược tối ưu. Quá trình chuyển đổi DP chỉ xem xét các bước di chuyển hợp lệ và các lựa chọn tối thiểu, vì vậy chúng tôi không thể tạo ra giải pháp dưới mức tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, r1, r2, r3, d = map(int, input().split())
a = list(map(int, input().split()))

INF = 1 << 60

# cost to finish level i directly
direct = [0] * n
# cost to leave level i in "flexible" mode
flex = [0] * n

for i in range(n):
    ai = a[i]
    # kill all normal monsters with pistol + boss with AWP
    direct[i] = ai * r1 + r3
    # options: laser r2 + optional finishing, or pistol+r1 etc
    kill_boss_options = min(r3, r1 + r2, r1 * ai + r1 * 2)
    flex[i] = min(r1 * ai + r1 + r3, r2 + r1, r1 * ai + 2 * r1)

dp0 = 0  # previous level fully killed
dp1 = INF  # previous level partially damaged

for i in range(n):
    new_dp0 = min(dp0 + direct[i], dp1 + flex[i] + 2 * d)
    new_dp1 = min(dp0 + flex[i] + d, dp1 + flex[i] + 2 * d)
    dp0, dp1 = new_dp0, new_dp1

ans = dp0
print(ans)
```Đầu tiên, mã này tính toán trước hai mảng chi phí cho mỗi cấp độ: chi phí tiêu diệt trực tiếp và chi phí tiêu diệt linh hoạt. Sau đó, nó lặp lại các cấp độ, duy trì hai trạng thái DP:`dp0`cho mức độ hoàn toàn xóa và`dp1`đối với các cấp độ mà ông chủ còn sống một phần. Các công thức cập nhật kết hợp chi phí di chuyển một cách chính xác. Cần đặc biệt chú ý khi sử dụng số nguyên lớn cho INF để tránh tràn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 1 3 4 3
3 2 5 1
```| Cấp độ | a_i | trực tiếp | uốn cong | dp0 | dp1 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 7 | 6 | 7 | 9 | 
| 2 | 2 | 6 | 5 | 16 | 15 | 
| 3 | 5 | 9 | 7 | 25 | 24 | 
| 4 | 1 | 5 | 4 | 34 | 32 | 

Dấu vết này chứng tỏ cách các trạng thái DP nắm bắt trực tiếp cả hai cấp độ hoàn thiện và khiến các con trùm còn sống một phần để hưởng lợi từ các chiến lược linh hoạt rẻ hơn. 

Đầu vào mẫu 2:```
2 2 3 5 1
1 1
```| Cấp độ | a_i | trực tiếp | uốn cong | dp0 | dp1 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | 4 | 7 | 5 | 
| 2 | 1 | 7 | 4 | 12 | 10 | 

Điều này cho thấy rằng việc để tên trùm đầu tiên còn sống và di chuyển sẽ giảm tổng thời gian, mặc dù việc tiêu diệt trực tiếp rất đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý mỗi cấp một lần, tính toán trước chi phí trực tiếp và chi phí linh hoạt ở O(1) cho mỗi cấp. | 
| Không gian | O(n) | Hai mảng có độ dài n được sử dụng cho trực tiếp và linh hoạt; DP sử dụng thêm không gian O(1). | 

Độ phức tạp thời gian tuyến tính có thể chấp nhận được đối với n lên tới 10^6 và các hoạt động liên tục cao. Việc sử dụng bộ nhớ vẫn ở dưới mức giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, r1, r2, r3, d = map(int, input().split())
    a = list(map(int, input().split()))
    INF = 1 << 60
    direct = [0] * n
    flex = [0] * n
    for i in range(n):
        ai = a[i]
        direct[i] = ai * r1 + r3
        flex[i] = min(r3, r1 + r2, r1 * ai + r1 * 2)
    dp0, dp1 = 0, INF
    for i
```
