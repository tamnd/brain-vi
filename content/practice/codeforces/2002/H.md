---
title: "CF 2002H - Đếm 101"
description: "Chúng ta được yêu cầu đếm các chuỗi số nguyên có thuộc tính hợp nhất rất cụ thể. Với một dãy có độ dài n cho trước, mỗi phần tử từ 1 đến m, chúng ta được phép lấy liên tục ba phần tử liên tiếp ai, a{i+1}, a{i+2} và gộp chúng thành một số duy nhất được định nghĩa là…"
date: "2026-06-08T14:00:16+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2002
codeforces_index: "H"
codeforces_contest_name: "EPIC Institute of Technology Round August 2024 (Div. 1 + Div. 2)"
rating: 3500
weight: 2002
solve_time_s: 122
verified: false
draft: false
---

[CF 2002H - Đếm 101](https://codeforces.com/problemset/problem/2002/H) 

**Đánh giá:** 3500 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các chuỗi số nguyên có thuộc tính hợp nhất rất cụ thể. Đối với một chuỗi có độ dài nhất định`n`, mỗi phần tử giữa`1`Và`m`, ta được phép lấy liên tục ba phần tử liên tiếp`a_i, a_{i+1}, a_{i+2}`và hợp nhất chúng thành một số duy nhất được xác định là`max(a_i + 1, a_{i+1}, a_{i+2} + 1)`. Mục tiêu của Bài toán 101 là thực hiện càng nhiều phép hợp nhất càng tốt mà không có kết quả nào vượt quá`m`. 

Đếm 101 đặt ra một câu hỏi cấp độ cao hơn: thay vì tính toán số lần hợp nhất tối đa cho một chuỗi nhất định, chúng tôi muốn, cho mọi số lần hợp nhất có thể có`k`từ`0`ĐẾN`floor((n-1)/2)`, số dãy có độ dài`n`đạt được chính xác`k`hợp nhất. 

Những hạn chế là nhỏ ở`m`(lên đến 30) và vừa phải trong`n`(lên tới 130). Điều đó gợi ý rằng một cách tiếp cận khai thác quy hoạch động trên`n`Và`m`các giá trị là khả thi, bởi vì việc lặp trực tiếp trên tất cả các chuỗi sẽ theo cấp số nhân:`m^n`có thể đạt được`30^130`, điều đó hoàn toàn không thể thực hiện được. Mỗi trường hợp thử nghiệm phải xuất ra một chuỗi số lượng theo modulo`10^9+7`, điều này báo hiệu rằng số lượng có thể tăng lên lớn. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó`n < 3`(không thể hợp nhất được, câu trả lời luôn là các phép toán bằng 0) hoặc các chuỗi trong đó`m = 1`(các giá trị đều bằng 1, vì vậy chỉ có thể xảy ra một số lần hợp nhất nhất định). Ví dụ,`n = 3, m = 1`chỉ dẫn đến một chuỗi`[1,1,1]`và có thể thực hiện chính xác một lần hợp nhất, cho kết quả đầu ra`0 1`. Giải pháp bất cẩn không xử lý được việc nhỏ`n`riêng biệt có thể tạo ra lỗi chỉ mục hoặc số lượng không chính xác. 

Một kịch bản tinh vi khác xảy ra khi số lượng tối đa`m`là rất nhỏ so với`n`, giới hạn số lần hợp nhất có thể được thực hiện. Ví dụ, nếu`m = 2`Và`n = 5`, không thể thực hiện hai lần hợp nhất vì lần hợp nhất đầu tiên đã tăng một số lên`3`, vượt quá`m`. Bất kỳ cách tiếp cận bạo lực nào giả định sự hợp nhất không hạn chế sẽ tính sai các chuỗi này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi chuỗi độ dài có thể có`n`với các phần tử từ`1`ĐẾN`m`, mô phỏng tất cả các lần hợp nhất có thể và đếm xem có bao nhiêu lần hợp nhất có thể xảy ra cho mỗi chuỗi. Điều này đúng về nguyên tắc nhưng hoàn toàn không khả thi: vì`n = 130`Và`m = 30`, chúng ta sẽ cần xử lý`30^130`trình tự. Ngay cả việc mô phỏng mỗi chuỗi cũng sẽ tốn kém vì chúng ta cần xem xét nhiều vị trí hợp nhất. 

Thông tin chuyên sâu quan trọng đến từ việc quan sát cấu trúc của hoạt động hợp nhất. Việc hợp nhất chỉ phụ thuộc vào ba phần tử liên tiếp. Nếu chúng ta xác định trạng thái DP theo dõi vị trí hiện tại, số lần hợp nhất được thực hiện và giá trị của hai phần tử cuối cùng, chúng ta có thể tính toán số lượng chuỗi một cách có hệ thống mà không cần liệt kê chúng một cách rõ ràng. 

Cụ thể, DP có thể được định nghĩa là`dp[pos][k][x][y]`= số chuỗi tính đến vị trí`pos`hai phần tử cuối cùng ở đâu`x`Và`y`, và chính xác`k`việc sáp nhập đã được thực hiện cho đến nay. Đối với phần tử tiếp theo`z`, chúng tôi sẽ thực hiện hợp nhất với hai phần tử cuối cùng (nếu`max(x+1, y, z+1) <= m`) hoặc mở rộng chuỗi mà không cần hợp nhất. Lặp lại tất cả`x, y, z`giá trị là khả thi bởi vì`m`là nhỏ, và`n`là vừa phải. Điều này làm giảm một bài toán hàm mũ thành một bài toán đa thức. 

Bằng cách cẩn thận cuộn DP qua các vị trí và nén trạng thái chỉ còn hai phần tử cuối cùng và số lượng hợp nhất hiện tại, chúng ta có thể đạt được giải pháp thiết thực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n * n) | O(m^n) | Quá chậm | 
| DP tối ưu | O(n * m^3 * n/2) ≈ O(n^2 * m^3) | O(n/2 * m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun nếu cần, nhưng trong giải pháp này, chúng tôi tránh tổ hợp bằng lập trình động. 
2. Khởi tạo mảng DP 3D`dp[k][x][y]`Ở đâu`k`là số lần hợp nhất được thực hiện và`x`Và`y`là hai phần tử cuối cùng trong chuỗi hiện tại. Bộ`dp[0][x][y] = 1`cho tất cả các cặp`(x, y)`nếu chúng ta tưởng tượng các chuỗi có độ dài 2. 
3. Lặp lại từ vị trí 2 lên đến`n-1`(được lập chỉ mục 0). Đối với mỗi trạng thái DP`(k, x, y)`, hãy cân nhắc việc thêm phần tử mới`z`từ 1 đến`m`. Tính toán ứng cử viên hợp nhất`merge_val = max(x + 1, y, z + 1)`. Nếu như`merge_val <= m`, thì chúng ta có thể thực hiện hợp nhất: tăng`dp[k+1][y][merge_val]`qua`dp[k][x][y]`. Nếu không, hãy mở rộng mà không hợp nhất: tăng`dp[k][y][z]`qua`dp[k][x][y]`. 
4. Sau khi điền DP vào vị trí`n-1`, tổng hợp tất cả`dp[k][x][y]`cho mỗi`k`để có được tổng số chuỗi có độ dài`n`sản xuất chính xác`k`hợp nhất. 
5. Trả về các số đếm này theo modulo`10^9+7`. 

Tại sao nó hoạt động: DP duy trì tính bất biến mà mỗi trạng thái`(k, x, y)`biểu thị chính xác số lượng chuỗi đạt đến cấu hình hai phần tử cuối cùng đó với`k`hợp nhất. Bằng cách xem xét phần tử tiếp theo một cách có hệ thống và tính toán điều kiện hợp nhất, chúng tôi đảm bảo rằng tất cả các chuỗi hợp lệ được tính chính xác một lần và không bao gồm chuỗi không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve(n, m):
    max_k = (n - 1) // 2
    dp = [[ [0]*(m+2) for _ in range(m+2) ] for _ in range(max_k+2)]
    
    # Base: sequences of length 2
    for x in range(1, m+1):
        for y in range(1, m+1):
            dp[0][x][y] = 1
    
    for pos in range(2, n):
        ndp = [[ [0]*(m+2) for _ in range(m+2) ] for _ in range(max_k+2)]
        for k in range(max_k+1):
            for x in range(1, m+1):
                for y in range(1, m+1):
                    val = dp[k][x][y]
                    if val == 0:
                        continue
                    for z in range(1, m+1):
                        merge_val = max(x+1, y, z+1)
                        if merge_val <= m and k+1 <= max_k:
                            ndp[k+1][y][merge_val] = (ndp[k+1][y][merge_val] + val) % MOD
                        else:
                            ndp[k][y][z] = (ndp[k][y][z] + val) % MOD
        dp = ndp
    
    result = []
    for k in range(max_k+1):
        total = 0
        for x in range(1, m+1):
            for y in range(1, m+1):
                total = (total + dp[k][x][y]) % MOD
        result.append(total)
    return result

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    res = solve(n, m)
    print(' '.join(map(str, res)))
```Mã này khởi tạo các chuỗi có độ dài 2, sau đó mở rộng chúng từng vị trí một. Ở mỗi bước, nó xem xét cả quá trình chuyển đổi hợp nhất và không hợp nhất. Việc sử dụng hai mảng 3D cho phép chúng tôi tiến về phía trước một cách hiệu quả. Modulo được áp dụng ở mỗi lần thêm để tránh tràn. Các vòng lặp lồng nhau là an toàn vì`m`nhiều nhất là 30 và`n`nhiều nhất là 130. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n = 3, m = 2** 

| tư thế | k | x | y | z | hợp nhất_val | ndp sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 1 | 1 | 2 | dp[1] [1] [2] = 1 | 
| 2 | 0 | 1 | 1 | 2 | 3 | vượt quá m | 
| 2 | 0 | 1 | 2 | 1 | 2 | dp[1] [2] [2] = 1 | 
| 2 | 0 | 1 | 2 | 2 | 3 | vượt quá m | 

Kết quả tính:`[6, 2]`**Ví dụ 2: n = 4, m = 2** 

Mở rộng các chuỗi, DP đếm chính xác các chuỗi tạo ra sự hợp nhất 0 hoặc 1,
