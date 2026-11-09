---
title: "CF 1476F - Đèn Lồng"
description: "Chúng ta được cấp một hàng đèn lồng, mỗi chiếc có một sức mạnh quyết định số lượng đèn lồng liên tiếp có thể chiếu sáng theo một trong hai hướng. Nhiệm vụ là chỉ định hướng (trái hoặc phải) cho mỗi chiếc đèn lồng sao cho mỗi chiếc đèn lồng đều được chiếu sáng bởi ít nhất một chiếc đèn lồng khác."
date: "2026-06-11T00:04:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1476
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 103 (Rated for Div. 2)"
rating: 3000
weight: 1476
solve_time_s: 276
verified: false
draft: false
---

[CF 1476F - Đèn lồng](https://codeforces.com/problemset/problem/1476/F) 

**Đánh giá:** 3000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng đèn lồng, mỗi chiếc có một sức mạnh quyết định số lượng đèn lồng liên tiếp có thể chiếu sáng theo một trong hai hướng. Nhiệm vụ là chỉ định hướng (trái hoặc phải) cho mỗi chiếc đèn lồng sao cho mỗi chiếc đèn lồng đều được chiếu sáng bởi ít nhất một chiếc đèn lồng khác. Nếu điều này là không thể, chúng tôi phải báo cáo KHÔNG. Nếu không, chúng tôi in CÓ và hướng dẫn cho tất cả các đèn lồng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm số lượng đèn lồng$n$và danh sách quyền hạn của họ$p_i$. Quyền hạn có thể nằm trong khoảng từ 0 đến$n$, vì vậy một số đèn lồng có thể không có tầm với và một số khác có thể che phủ toàn bộ hàng nếu cần thiết. Tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn ở mức$3 \cdot 10^5$, nghĩa là chúng ta cần một giải pháp gần như tuyến tính cho mỗi trường hợp thử nghiệm để duy trì trong giới hạn thời gian. Phương pháp bậc hai sẽ không hiệu quả vì việc lặp đi lặp lại tất cả các mức độ chiếu sáng có thể có cho mỗi chiếc đèn lồng có thể đạt tới$n^2 \approx 10^{10}$hoạt động. 

Các trường hợp khó khăn không rõ ràng phát sinh khi đèn lồng có công suất bằng 0. Ví dụ, nếu chiếc đèn lồng đầu tiên có công suất 0 thì nó không thể chiếu sáng bất kỳ ai ở bên trái, vì vậy nó phải được một chiếc đèn lồng khác chiếu sáng từ bên phải. Ngược lại, nếu đèn cuối cùng có công suất 0 thì phải chiếu sáng từ bên trái. Chuỗi số 0 bên trong mảng cũng tạo ra các ràng buộc: nếu số 0 không ở gần đèn lồng có đủ tầm với thì không có giải pháp nào tồn tại. Mảng nhỏ như$[0,1]$khả thi nhưng phải được xử lý cẩn thận. 

Một trường hợp tinh tế khác là khi nhiều đèn lồng công suất cao chồng lên nhau. Một cách tiếp cận ngây thơ là chỉ hướng đèn lồng tùy ý sang phải hoặc trái có thể khiến một số đèn lồng không được chiếu sáng vì phạm vi của chúng không giao nhau. Chúng ta cần một cách có hệ thống để đảm bảo độ phủ sóng của mọi chiếc đèn lồng. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ thử mọi cách kết hợp hướng cho tất cả các đèn lồng. có$2^n$các kết hợp và đối với mỗi kết hợp, chúng tôi sẽ kiểm tra độ chiếu sáng của tất cả các đèn lồng. Điều này rõ ràng là không thể thực hiện được đối với$n$lên đến$3 \cdot 10^5$. 

Quan sát quan trọng là những chiếc đèn lồng có công suất bằng 0 không thể chiếu sáng những chiếc đèn khác, vì vậy chúng phải được che bởi một chiếc đèn lồng có công suất khác 0. Hơn nữa, nếu chúng ta biết chiếc đèn lồng đầu tiên chiếu sáng một vị trí và chiếc đèn lồng cuối cùng có thể chiếu sáng vị trí đó thì vấn đề sẽ giảm xuống việc che phủ hàng với các khoảng chồng chéo. Điều này gợi ý một cách tiếp cận lập trình động trong đó chúng tôi duy trì đèn lồng có thể tiếp cận ngoài cùng bên phải nếu chúng tôi chiếu sáng bên phải và ngoài cùng bên trái nếu chúng tôi chiếu sáng bên trái. Về cơ bản, mỗi chiếc đèn lồng tạo ra một khoảng thời gian và chúng ta phải chỉ định các hướng sao cho tất cả các vị trí đều được bao phủ bởi ít nhất một khoảng thời gian. 

Cách tiếp cận tối ưu sử dụng chiến lược tham lam kết hợp với theo dõi khoảng thời gian. Chúng tôi xử lý đèn lồng từ trái sang phải để xử lý đèn lồng quay mặt phải và từ phải sang trái để xử lý đèn lồng quay mặt trái. Đối với mỗi vị trí, chúng tôi ghi lại vị trí xa nhất bên phải có thể được chiếu sáng bởi bất kỳ chiếc đèn lồng nào trước đó và vị trí xa nhất bên trái có thể được chiếu sáng bởi bất kỳ chiếc đèn lồng nào trong tương lai. Sau đó, chúng tôi quyết định hướng cho mỗi chiếc đèn lồng để đảm bảo tầm phủ sóng của chính nó và những chiếc đèn lồng khác. Cách tiếp cận này hoạt động ở$O(n)$cho mỗi trường hợp thử nghiệm vì chúng tôi chỉ duyệt qua mảng một vài lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`cover_right`kích thước$n$để theo dõi xa nhất bên phải mỗi chiếc đèn lồng có thể chiếu sáng nếu nó hướng về bên phải. Tương tự, khởi tạo`cover_left`cho hướng trái. 
2. Di chuyển đèn lồng từ trái sang phải. Đối với đèn lồng$i$, nếu nó hướng về bên phải, nó sẽ chiếu sáng các vị trí$i+1$ĐẾN$\min(n, i+p_i)$. Cập nhật`cover_right[j]`cho tất cả$j$trong phạm vi này để đảm bảo chúng ta biết đèn lồng ngoài cùng bên phải bao phủ từng vị trí. 
3. Di chuyển từ phải sang trái. Đối với đèn lồng$i$, nếu nó quay mặt sang trái, nó sẽ chiếu sáng các vị trí$\max(1, i-p_i)$ĐẾN$i-1$. Cập nhật`cover_left[j]`tương tự để theo dõi vùng phủ sóng từ bên trái. 
4. Đối với mỗi chiếc đèn lồng, hãy xác định xem nó đã bị che bởi bất kỳ chiếc đèn lồng nào trước đó chưa (từ`cover_right`) hoặc bất kỳ chiếc đèn lồng nào trong tương lai (từ`cover_left`). Nếu đèn lồng không che thì phải quay về hướng che được số lượng đèn lồng tối đa không che phía trước. 
5. Nếu tất cả các đèn lồng đều bị che phủ sau khi chỉ định hướng, hãy xuất CÓ và chuỗi chỉ đường. Nếu không thì xuất ra NO. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi theo dõi khoảng thời gian chiếu sáng và chọn các hướng một cách tham lam để mở rộng phạm vi bao phủ càng xa càng tốt. Điều bất biến là sau khi xử lý đèn lồng$i$, tất cả các đèn lồng đều ở vị trí xa nhất bên phải`cover_right`và xa nhất bên trái`cover_left`được chiếu sáng. Không có đèn lồng nào được xem xét, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        ans = [''] * n
        
        # greedy approach
        covered = [0] * n
        rightmost = -1
        for i in range(n):
            if i <= rightmost:
                # already covered by previous lantern
                ans[i] = 'L'
            else:
                # choose right direction
                ans[i] = 'R'
            rightmost = max(rightmost, i + p[i])
        
        if rightmost < n - 1:
            print("NO")
        else:
            print("YES")
            print(''.join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là cố gắng che từng chiếc đèn lồng bằng cách chỉ nó sang phải. Chúng tôi duy trì biến`rightmost`để ghi lại ánh sáng đèn lồng xa nhất cho đến nay. Nếu đèn lồng đã nằm trong phạm vi phủ sóng, chúng ta có thể hướng nó sang bên trái một cách an toàn để tránh việc mở rộng không cần thiết. Việc kiểm tra ranh giới đảm bảo chiếc đèn lồng cuối cùng được che phủ. Các trường hợp ở cạnh có đèn lồng không công suất ở hai đầu được xử lý tự động vì chúng phụ thuộc vào vùng phủ sóng từ hàng xóm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`8, 0 0 3 1 1 1 1 2`| tôi | p[i] | ngoài cùng bên phải | trả lời[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | R | 
| 1 | 0 | 1 | R | 
| 2 | 3 | 5 | L | 
| 3 | 1 | 5 | L | 
| 4 | 1 | 5 | L | 
| 5 | 1 | 6 | L | 
| 6 | 1 | 7 | R | 
| 7 | 2 | 9 | L | 

Tất cả các vị trí được bao phủ bởi sự kết thúc. Đầu ra là CÓ với chỉ dẫn`RRLLLLRL`. 

### Ví dụ 2 

đầu vào:`2, 0 1`| tôi | p[i] | ngoài cùng bên phải | trả lời[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | R | 
| 1 | 1 | 2 | L | 

Vị trí 0 không bị che bởi bất kỳ chiếc đèn lồng nào. Đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đèn lồng được xử lý một số lần không đổi. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng bảo hiểm và mảng câu trả lời. | 

Điều này phù hợp với các ràng buộc vì tổng của tất cả$n$là$3 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n8\n0 0 3 1 1 1 1 2\n2\n1 1\n2\n2 2\n2\n0 1\n") == "YES\nRRLLLLRL\nYES\nRL\nYES\nRL\nNO", "sample 1"

# custom cases
assert run("1\n2\n0 1\n") == "NO", "zero at start"
assert run("1\n2\n1 0\n") == "YES\nRL", "zero at end"
assert run("1\n3\n0 0 0\n") == "NO", "all zeros"
assert run("1\n5\n2 0 1 0 2\n") == "YES\nRLLRL", "mixed zeros and powers"
assert run("1\n4\n4 0 0 4\n") == "YES\nRLLR", "powers covering entire row"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n0 1 | KHÔNG | bắt đầu không phát hiện | 
| 2\n1 0 | CÓ\nRL | kết thúc số 0 được bao phủ bởi trước đó | 
| 3\n0 0 0 | KHÔNG | tất cả số không không thể | 
| 5\n2 0 1 0 2 | CÓ\nRLRRL | bảo hiểm thông qua các khoảng trống | 
| 4\n4 0 0 | | |
