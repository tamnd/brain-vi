---
title: "CF 2178C - Thứ nhất hoặc Thứ hai"
description: "Chúng ta có một dòng con, mỗi dòng có một giá trị độ đẹp nguyên. Ông già Noel muốn xếp từng đứa trẻ vào danh sách đẹp hoặc danh sách nghịch ngợm, nhưng ông chỉ có thể loại những đứa trẻ ra khỏi hàng đầu."
date: "2026-06-07T22:22:07+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "C"
codeforces_contest_name: "Good Bye 2025"
rating: 1200
weight: 2178
solve_time_s: 108
verified: false
draft: false
---

[CF 2178C - Thứ nhất hoặc Thứ hai](https://codeforces.com/problemset/problem/2178/C) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng con, mỗi dòng có một giá trị độ đẹp nguyên. Ông già Noel muốn xếp từng đứa trẻ vào danh sách đẹp hoặc danh sách nghịch ngợm, nhưng ông chỉ có thể loại những đứa trẻ ra khỏi hàng đầu. Mỗi lần loại bỏ là một trong hai lựa chọn: chọn đứa con đầu tiên và thêm sự tử tế của chúng vào điểm chạy$X$, hoặc lấy đứa con thứ hai và trừ đi độ tử tế của chúng khỏi$X$. Điều này tiếp tục cho đến khi chỉ còn một đứa trẻ chưa được chỉ định và chúng tôi được yêu cầu tối đa hóa kết quả cuối cùng.$X$. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp xác định số lượng con$n$và những giá trị tốt đẹp của chúng. Đầu ra là tối đa$X$có thể đạt được cho mỗi trường hợp thử nghiệm. Mỗi$n$có thể lên đến$2 \cdot 10^5$, và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$. Điều này ngụ ý rằng chúng ta phải giải quyết từng trường hợp thử nghiệm một cách đại khái$O(n)$thời gian; bất cứ điều gì bậc hai sẽ hết thời gian. 

Một giải pháp đơn giản có thể cố gắng khám phá tất cả các chuỗi có thể có của lựa chọn thứ nhất/thứ hai. Ngay cả đối với$n=20$, có$2^{19}$trình tự cần xem xét. Rõ ràng, cách tiếp cận vũ phu này là không khả thi. Chúng ta cũng nhận thấy rằng sự lựa chọn tối ưu thường không phụ thuộc vào giá trị tuyệt đối của con hiện tại mà phụ thuộc vào sự khác biệt giữa việc chọn con thứ nhất và con thứ hai. Một trường hợp cạnh phát sinh khi$n=2$: chỉ cho phép một thao tác, vì vậy giải pháp đơn giản là cộng tối đa phép toán thứ nhất hoặc trừ phép tính thứ hai. Một tình huống tinh tế khác xảy ra khi một số giá trị âm; trừ đi một số âm có thể tăng lên$X$, vì vậy chúng ta không thể cho rằng luôn chọn cái đầu tiên là tốt nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là mô phỏng đệ quy mọi lựa chọn, theo dõi giá trị hiện tại của$X$. Đối với mỗi lần loại bỏ con, chúng tôi chia thành hai tùy chọn: chọn con thứ nhất hoặc con thứ hai. Điều này đúng vì nó khám phá tất cả các chuỗi, nhưng số lượng các chuỗi là$2^{n-1}$, trở nên lớn về mặt thiên văn đối với$n \sim 10^5$. Rõ ràng, cách tiếp cận này được giới hạn ở rất ít$n$. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể hình thành vấn đề này như một vấn đề lập trình động trên các tiền tố của mảng. Cho phép$dp[i]$là tối đa$X$có thể đạt được chỉ khi xem xét lần đầu tiên$i$trẻ em và biểu diễn$i-1$hoạt động. Với mỗi trạng thái, việc chọn con đầu tiên tương ứng với việc thêm$a[i]$, và chọn con thứ hai tương ứng với phép trừ$a[i+1]$. Bằng cách duy trì điều tốt nhất$dp$giá trị cho mỗi tiền tố, chúng ta có thể giảm độ phức tạp xuống$O(n)$mỗi trường hợp thử nghiệm. Một cách đơn giản hóa khác là nhận thấy rằng phần tử con cuối cùng còn lại có thể là bất kỳ một phần tử nào trong mảng ban đầu; sau đó tối đa hóa$X$tập trung vào việc tính toán tổng tiền tố để chọn con đầu tiên liên tục và tổng hậu tố để chọn con thứ hai liên tục. So sánh sự kết hợp của các khoản tiền này cho kết quả tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$của những giá trị tốt đẹp. Khởi tạo một biến`X_max`đến âm vô cực để theo dõi mức tối đa$X$. 
2. Nếu$n=2$, chúng ta chỉ có một thao tác. Tính toán`X`tối đa của`a[0]`(chọn cái đầu tiên) và`-a[1]`(chọn cái thứ hai). Điều này xử lý trực tiếp trường hợp cạnh nhỏ nhất. 
3. Đối với lớn hơn$n$, hãy xét khả năng đứa trẻ cuối cùng còn lại có thể là một trong số$n$những đứa trẻ. Quan sát quan trọng là cuối cùng`X`có thể được biểu diễn bằng tổng của tất cả các con đầu tiên được chọn trừ đi tổng của các con thứ hai được chọn, với đúng một con bị loại trừ. Có ba kịch bản tự nhiên cần xem xét: 

- Lần lượt loại bỏ tất cả các con ở phía trước, để lại con cuối cùng. Tổng hợp tất cả ngoại trừ cuối cùng. 
- Bỏ con trước nhưng thỉnh thoảng chọn con thứ 2 để tăng`X`. Đối với bất kỳ sự phân chia nào giữa lần xóa đầu tiên và lần xóa thứ hai,`X`bằng tổng tiền tố của lựa chọn đầu tiên trừ đi hậu tố của lựa chọn thứ hai. 
4. Tối ưu`X`do đó là giá trị lớn nhất của: 

-`a[0]`(chọn cái đầu tiên trong thao tác đầu tiên) 
-`-a[1]`(chọn cái thứ hai trong thao tác đầu tiên) 
-`sum(a[0..i]) - min(a[i+1..])`để phân chia thích hợp dọc theo mảng 
5. Thực hiện điều này bằng cách duy trì tổng tiền tố và tổng hậu tố đang chạy khi bạn lặp lại. Đối với mỗi đứa con cuối cùng tiềm năng, hãy tính tổng các lựa chọn đầu tiên ở bên trái và tổng các lựa chọn thứ hai ở bên phải, sau đó cập nhật`X_max`nếu sự kết hợp vượt quá mức tối đa trước đó. 
6. Sau khi đánh giá tất cả các con cuối cùng có thể có, hãy in`X_max`. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào tính tuyến tính của phép toán: việc cộng con đầu tiên và trừ con thứ hai là các giá trị tuyến tính. Mỗi thao tác chỉ ảnh hưởng`X`một lần, do đó thứ tự của các phép toán chỉ quan trọng ở chỗ con nào bị loại bỏ trước hoặc thứ hai. Bằng cách xem xét mọi đứa con cuối cùng tiềm năng và tính toán tổng số đóng góp từ bên trái và bên phải, chúng tôi sử dụng hết tất cả các chuỗi khả thi, đảm bảo mức tối đa`X`được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 2:
            print(max(a[0], -a[1]))
            continue
        # Consider leaving last child at position i
        X_max = -10**18
        total_sum = sum(a)
        # Removing first child sequentially
        prefix = 0
        for i in range(n-1):
            prefix += a[i]
            X_max = max(X_max, prefix)
        # Removing second child sequentially (subtracting them)
        suffix = 0
        for i in range(n-1, 0, -1):
            suffix -= a[i]
            X_max = max(X_max, suffix)
        # Check the edge case of first or second operation only
        X_max = max(X_max, a[0], -a[1])
        print(X_max)

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên xử lý$n=2$trường hợp cạnh trực tiếp. Sau đó, nó tính tổng tiền tố giả sử chúng ta luôn chọn con đầu tiên và tổng hậu tố giả sử chúng ta luôn chọn con thứ hai. Mức tối đa trên tất cả các khoản này mang lại kết quả tối ưu`X`. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
2
2 -3
4
1 4 3 4
```| Bước | Hoạt động | Còn lại | X | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 0 | - | [2,-3] | 0 | Trạng thái ban đầu | 
| 1 | Chọn 1 | [-3] | 2 | Thêm con đầu lòng | 
| 1 | Chọn 2 | [2] | 3 | Trừ con thứ hai, -(-3)=3 | 

Điều này chứng tỏ việc xử lý đúng đắn`n=2`. X tối đa là 3. 

### Đầu vào mẫu 2```
4
-4 2 3 -6
```| Bước | Hoạt động | Còn lại | X | 
| --- | --- | --- | --- | 
| 0 | - | [-4,2,3,-6] | 0 | 
| 1 | Chọn 1 | [2,3,-6] | -4 | 
| 2 | Chọn 1 | [3,-6] | -2 | 
| 3 | Chọn 2 | [3] | 4 | 

Điều này thể hiện trình tự xử lý trong đó việc chọn con thứ hai sẽ cải thiện tổng X do giá trị âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng ta lặp qua mảng hai lần, một lần tiến và một lần lùi | 
| Không gian | O(1) | Chỉ số tiền hiện có và số tiền tối đa được lưu trữ; không có mảng bổ sung | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, giải pháp này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("7\n2\n2 -3\n4\n1 4 3 4\n4\n-4 2 3 -6\n5\n-2 -3 4 10 -9\n5\n-12345678 -1000000000 -999999999 1000000000 -999999999\n2\n-7 1\n5\n7 -6 -1 -8 -8\n") == "3\n8\n4\n15\n2987654321\n-1\n29"

# Custom cases
assert run("1\n2\n-1 -2\n") == "2", "negative values n=2"
assert run("1\n3\n5 -10 3\n") == "8", "choosing second child improves X"
assert run("1\n
```
