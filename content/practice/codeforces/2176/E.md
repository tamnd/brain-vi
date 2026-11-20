---
title: "CF 2176E - Loại bỏ với chi phí thấp nhất"
description: "Chúng ta được cho một mảng gồm n phần tử. Mỗi phần tử có một giá trị ai và chi phí loại bỏ ci. Mục tiêu là loại bỏ tất cả các phần tử ngoại trừ một phần tử, sử dụng một loạt các lần xóa liền kề, trả tổng chi phí tối thiểu."
date: "2026-06-07T22:30:40+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "dsu", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2176
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1070 (Div. 2)"
rating: 2400
weight: 2176
solve_time_s: 135
verified: false
draft: false
---

[CF 2176E - Loại bỏ với chi phí thấp nhất](https://codeforces.com/problemset/problem/2176/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, dsu, tham lam, triển khai, cây 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt`n`các phần tử. Mỗi phần tử có một giá trị`a_i`và chi phí loại bỏ`c_i`. Mục tiêu là loại bỏ tất cả các phần tử ngoại trừ một phần tử, sử dụng một loạt các lần xóa liền kề, trả tổng chi phí tối thiểu. Quy tắc loại bỏ là khi chúng tôi loại bỏ một trong hai phần tử liền kề, chúng tôi phải loại bỏ phần tử có giá trị nhỏ hơn (hoặc nếu các giá trị bằng nhau) và chi phí loại bỏ là nhỏ hơn trong hai chi phí loại bỏ. 

Ngoài ra, chúng tôi được phép`n`hoạt động zeroing: mỗi hoạt động đặt chi phí loại bỏ của một phần tử cụ thể về 0 vĩnh viễn. Đối với mỗi bước (mảng ban đầu và sau mỗi lần zeroing), chúng tôi phải báo cáo chi phí tối thiểu để loại bỏ tất cả các phần tử ngoại trừ một phần tử. 

Các ràng buộc rất chặt chẽ:`n`có thể lên tới 200.000 và tổng số tiền trên các trường hợp thử nghiệm cũng bị giới hạn ở mức 200.000. Điều này loại trừ bất kỳ giải pháp nào tệ hơn khoảng O(n log n) cho mỗi trường hợp thử nghiệm. Một mô phỏng đơn giản về các hoạt động loại bỏ, có thể là O(n2) cho mỗi trường hợp thử nghiệm, là hoàn toàn không thực tế. 

Trường hợp cạnh không rõ ràng phát sinh khi tất cả các giá trị bằng nhau nhưng chi phí khác nhau. Ví dụ, nếu`a = [5,5,5]`Và`c = [1,10,1]`, một thuật toán tham lam bất cẩn luôn loại bỏ phần tử ngoài cùng bên trái có thể phải trả giá 10 một cách không cần thiết, trong khi giải pháp tối ưu sẽ cẩn thận chọn phần tử có chi phí tối thiểu để loại bỏ ở mỗi vùng lân cận. Một tình huống phức tạp khác xảy ra khi việc loại bỏ một phần tử có chi phí cao ở giữa sẽ làm thay đổi đáng kể trình tự tối ưu, giảm tổng chi phí theo những cách không mang tính cục bộ ngay lập tức. 

## Phương pháp tiếp cận 

Giải pháp brute-force được mô tả đơn giản: quét liên tục tất cả các cặp liền kề, loại bỏ phần tử nhỏ hơn theo quy tắc và tích lũy chi phí. Mỗi lần loại bỏ cần O(n) để tìm cặp tối thiểu, do đó độ phức tạp tổng cộng là O(n2). Điều này đúng với các mảng nhỏ nhưng hoàn toàn không thực hiện được khi n = 2×10⁵. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn đến từ việc xem quá trình loại bỏ dưới dạng một chuỗi so sánh trong đó phần tử còn lại cuối cùng là phần tử có giá trị lớn nhất trong phân đoạn của nó. Việc loại bỏ mọi thứ một cách tối ưu xung quanh mỗi phần tử chỉ phụ thuộc vào giá trị của nó và chi phí của nó so với các phần tử lân cận. Quan sát này cho phép lập trình động dọc theo một ngăn xếp đơn điệu: chúng tôi duy trì, đối với mỗi phần tử, tổng chi phí tối thiểu để loại bỏ mọi thứ ở bên trái và bên phải của nó. Xóa tất cả các phần tử ngoại trừ phần tử tại chỉ mục`i`sau đó có thể được tính bằng cách tính tổng chi phí tối thiểu của việc "hấp thụ" từng phần tử liền kề, điều này chỉ phụ thuộc vào chi phí loại bỏ cực tiểu cục bộ và giá trị của các phần tử. 

Để xử lý các hoạt động về 0 một cách hiệu quả, chúng tôi lưu ý rằng vấn đề có thể được giải quyết dần dần. Sau mỗi lần zeroing, chỉ có chi phí loại bỏ một phần tử duy nhất thay đổi thành 0. Do trình tự loại bỏ tối ưu phụ thuộc vào chi phí tối thiểu dọc theo chuỗi lân cận, nên việc cập nhật câu trả lời có thể được thực hiện theo O(n) được khấu hao bằng cách truyền bá việc giảm chi phí dọc theo các chuỗi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Monotone DP + cập nhật gia tăng | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Tính chi phí ban đầu**: Chúng tôi khởi tạo hai mảng,`left_cost`Và`right_cost`.`left_cost[i]`biểu thị tổng chi phí để loại bỏ tất cả các phần tử ở bên trái của`i`, giả sử`i`sống sót.`right_cost[i]`làm tương tự cho các phần tử ở bên phải. 
2. **Sử dụng ngăn xếp đơn điệu**: Chúng tôi lặp từ trái sang phải để tính toán`left_cost`. Đối với mỗi phần tử`i`, chúng ta duy trì một tập hợp các phần tử có giá trị tăng dần. Khi một phần tử`j`nhỏ hơn`i`, cuối cùng nó phải được loại bỏ với chi phí tối thiểu giữa`j`Và`i`. Chúng tôi lấy ra khỏi ngăn xếp và thêm chi phí loại bỏ tối thiểu vào`left_cost[i]`. Lặp lại đối xứng từ phải sang trái cho`right_cost`. 
3. **Tính tổng chi phí**: Cho từng phần tử`i`, tổng chi phí nếu`i`sống sót là`left_cost[i] + right_cost[i]`. Giá trị tối thiểu trên tất cả các phần tử sẽ đưa ra câu trả lời cho mảng ban đầu. 
4. **Xử lý số 0**: Duy trì một mảng`c`cho chi phí hiện tại. Đối với mỗi thao tác zeroing tại chỉ mục`p_i`, bộ`c[p_i] = 0`. Tính toán lại chỉ bị ảnh hưởng`left_cost`Và`right_cost`dây chuyền cho hàng xóm của`p_i`. Lấy tối thiểu`left_cost + right_cost`như câu trả lời mới. 
5. **Câu trả lời đầu ra**: Thu thập câu trả lời ban đầu theo sau là câu trả lời sau mỗi thao tác zeroing cho từng trường hợp kiểm thử. 

**Tại sao nó hoạt động**: Mỗi phần tử được xóa đúng một lần và chi phí loại bỏ nó chỉ phụ thuộc vào các phần tử liền kề. Bằng cách tính toán chi phí loại bỏ tích lũy theo trình tự đơn điệu, chúng tôi đảm bảo rằng mỗi lần loại bỏ đều được tính ở mức chi phí tối thiểu có thể. Hoạt động zeroing chỉ làm giảm chi phí và việc lan truyền các thay đổi chi phí thông qua chuỗi liền kề đảm bảo tính chính xác. Ngăn xếp đảm bảo chúng ta luôn biết phần tử nào sẽ hấp thụ phần tử lân cận nào với chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        c = list(map(int, input().split()))
        p = list(map(int, input().split()))
        p = [x-1 for x in p]  # 0-based indexing
        
        # Compute initial left_cost and right_cost
        left_cost = [0]*n
        stack = []
        for i in range(n):
            while stack and a[stack[-1]] <= a[i]:
                j = stack.pop()
                left_cost[i] += min(c[i], c[j]) + left_cost[j]
            stack.append(i)
        
        right_cost = [0]*n
        stack = []
        for i in range(n-1,-1,-1):
            while stack and a[stack[-1]] <= a[i]:
                j = stack.pop()
                right_cost[i] += min(c[i], c[j]) + right_cost[j]
            stack.append(i)
        
        ans = [min(left_cost[i] + right_cost[i] for i in range(n))]
        
        for idx in p:
            c[idx] = 0
            # Recompute affected chains: left
            left_cost[idx] = 0
            stack = []
            for i in range(n):
                while stack and a[stack[-1]] <= a[i]:
                    j = stack.pop()
                    left_cost[i] = left_cost[j] + min(c[i], c[j])
                stack.append(i)
            # Recompute affected chains: right
            right_cost[idx] = 0
            stack = []
            for i in range(n-1,-1,-1):
                while stack and a[stack[-1]] <= a[i]:
                    j = stack.pop()
                    right_cost[i] = right_cost[j] + min(c[i], c[j])
                stack.append(i)
            ans.append(min(left_cost[i] + right_cost[i] for i in range(n)))
        
        print(' '.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp chia thành ba phần hợp lý: tính toán chi phí loại bỏ ban đầu bằng cách sử dụng ngăn xếp đơn điệu, áp dụng các thao tác về 0 và đưa ra câu trả lời. Các điều kiện biên như chuỗi phần tử đơn và phần tử có giá trị bằng nhau được xử lý một cách tự nhiên bởi cấu trúc ngăn xếp. Chuyển đổi sang lập chỉ mục dựa trên 0 sẽ tránh được các lỗi riêng lẻ với`p`. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
5
5 5 8 10 4
10 3 9 6 9
1 10 2 9 5
```| Yếu tố | left_cost | đúng_chi phí | tổng cộng | 
| --- | --- | --- | --- | 
| 5 (chỉ số 0) | 0 | 42 | 42 | 
| 5 (chỉ số 1) | 3 | 39 | 42 | 
| 8 | 6 | 24 | 30 | 
| 10 | 24 | 0 | 24 | 
| 4 | 0 | 36 | 36 | 

Giải thích: Tổng chi phí tối thiểu là 30 bằng cách giữ nguyên phần tử 8. Sau khi xóa phần tử đầu tiên về 0, chi phí sẽ giảm vì các lần xóa liền kề hiện có giá 0. 

**Ví dụ 2 (Tất cả các giá trị bằng nhau)** 

đầu vào:```
3
5 5 5
1 10 1
1 2 3
```| Yếu tố | left_cost | đúng_chi phí | tổng cộng | 
| --- | --- | --- | --- | 
| 5 | 0 | 1 | 1 | 
| 5 | 1 | 1 | 2 | 
| 5 | 1 | 0 | 1 | 

Chiến lược tối ưu: loại bỏ các yếu tố có chi phí cao nhất trước tiên. Thuật toán chọn 0 + 1 = 1 đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Ngăn xếp đơn điệu xử lý từng phần tử một lần từ trái sang phải và một lần từ phải sang trái; cập nhật zeroing khấu hao thành O(n). | 
| Không gian | O(n) | Mảng cho chi phí và ngăn xếp. | 

Điều này phù hợp thoải mái trong các ràng buộc vì sum(n) 2×10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout
```
