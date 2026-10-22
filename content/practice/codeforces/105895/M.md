---
title: "CF 105895M - \u732b\u5a18\u90e8\u7f72"
description: "Chúng tôi được cung cấp một lưới nhỏ, tối đa 8 x 8, trong đó mỗi ô được phép hoặc bị cấm. Các ô được phép chứa một con mèo và được đánh dấu bằng ký tự như Y, trong khi các ô bị cấm được đánh dấu bằng N."
date: "2026-06-21T15:15:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105895
codeforces_index: "M"
codeforces_contest_name: "The 21st Southeast University Programming Contest (Summer)"
rating: 0
weight: 105895
solve_time_s: 46
verified: true
draft: false
---

[CF 105895M - \u732b\u5a18\u90e8\u7f72](https://codeforces.com/problemset/problem/105895/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới nhỏ, tối đa 8 x 8, trong đó mỗi ô được phép hoặc bị cấm. Các ô được phép chứa một con mèo và được đánh dấu bằng một ký tự như`Y`, trong khi các ô bị cấm được đánh dấu bằng`N`. Nhiệm vụ là đặt càng nhiều miêu nữ càng tốt vào các ô được phép, với hạn chế là không có hai miêu nữ nào được đặt cạnh nhau theo chiều ngang hoặc chiều dọc. 

Về cơ bản, đây là bài toán tập hợp độc lập cực đại trên biểu đồ lưới với một số đỉnh bị loại bỏ. Mỗi ô là một đỉnh nếu nó có thể sử dụng được và các cạnh kết nối các ô liền kề trực giao. Chúng tôi muốn chọn tập hợp con lớn nhất của các đỉnh có thể sử dụng sao cho không có cạnh nào có cả hai điểm cuối được chọn. 

Ràng buộc n, m ≤ 8 là tín hiệu cấu trúc chính. Lưới có tối đa 64 ô, ngay lập tức loại trừ việc tìm kiếm theo cấp số nhân chung trên tất cả các tập hợp con của ô mà không cần cắt tỉa. Một bảng liệt kê tập hợp con ngây thơ sẽ xem xét tới 2^64 trạng thái, quá lớn ngay cả khi cắt tỉa mạnh mẽ. Đồng thời, độ rộng nhỏ gợi ý rõ ràng về lập trình động cấu hình hoặc DP mặt nạ bit trên các hàng hoặc cột. 

Một trường hợp tinh tế phá vỡ trực giác tham lam là khi các vị trí tối ưu cục bộ chặn các vùng dày đặc trong tương lai. Ví dụ: trong 2x3 tất cả-`Y`lưới, việc đặt một cách tham lam vào mẫu bàn cờ có thể bỏ sót các cấu hình làm thay đổi mật độ giữa các hàng. 

Một trường hợp góc khác là khi tất cả các ô đều`N`ngoại trừ cấu trúc thưa thớt như đường chéo. Bất kỳ giải pháp nào cũng phải tôn trọng rằng sự liền kề chỉ theo chiều ngang và chiều dọc, vì vậy các vị trí theo đường chéo không gây cản trở và tất cả đều phải được cho phép. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả các tập hợp con của các ô lưới và kiểm tra tính hợp lệ bằng cách quét tất cả các cặp hoặc kiểm tra các ràng buộc kề. Về mặt khái niệm, điều này đơn giản: chọn một tập hợp con, xác minh rằng không có hai ô được chọn nào liền kề nhau và đếm kích thước nếu hợp lệ. Tính đúng đắn rất đơn giản vì nó trực tiếp mã hóa định nghĩa của vấn đề. 

Tuy nhiên, số lượng tập hợp con tăng lên là 2^(n·m). Trong trường hợp xấu nhất của lưới 8 × 8, đây là 2^64, điều này hoàn toàn không khả thi. Ngay cả khi việc kiểm tra tính hợp lệ là O(64) cho mỗi tập hợp con thì tổng công việc vẫn rất lớn. 

Quan sát quan trọng là sự kề cận mang tính cục bộ và lưới hẹp. Thay vì quyết định tất cả các ô một cách độc lập, chúng tôi xử lý từng hàng và mã hóa từng lựa chọn hàng dưới dạng mặt nạ bit. Cấu hình hợp lệ cho một hàng là cấu hình không có hai ô được chọn nằm cạnh nhau theo chiều ngang. Sau đó, khả năng tương thích giữa các hàng liên tiếp buộc các ô liền kề theo chiều dọc không được chọn cả hai. 

Điều này biến vấn đề thành một hồ sơ DP trên các hàng, trong đó mỗi trạng thái là một mặt nạ bit hợp lệ cho một hàng và các chuyển đổi chỉ phụ thuộc vào các hàng liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^(nm) · nm) | O(nm) | Quá chậm | 
| Hàng DP với trạng thái mặt nạ bit | O(n · 2^m · 2^m) | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hàng một cách độc lập và thể hiện các lựa chọn trong một hàng bằng cách sử dụng mặt nạ bit có độ dài m. Một bit là 1 nếu chúng ta đặt một cô gái mèo vào cột đó. 

1. Đối với mỗi hàng, liệt kê tất cả các mặt nạ bit từ 0 đến 2^m − 1 và chỉ giữ lại những mặt nạ hợp lệ trong hàng. Một mặt nạ hợp lệ nếu nó không đặt hai catgirl vào các cột liền kề, nghĩa là không có hai bit liên tiếp nào bằng 1 và nó chỉ đặt các catgirl vào các ô chứa`Y`. Điều này đảm bảo chúng tôi không bao giờ vi phạm vùng lân cận theo chiều ngang hoặc vị trí trên các ô bị cấm. 
2. Tính toán trước, đối với mỗi hàng, mặt nạ nào tương thích với các ràng buộc lưới của hàng đó. Chúng tôi lưu trữ cho mỗi hàng một danh sách các mặt nạ hợp lệ và số lượng ô được chọn tương ứng của chúng. 
3. Xác định DP trong đó dp[row][mask] là số lượng catgirl tối đa mà chúng ta có thể đặt ở hàng này nếu chúng ta chọn mặt nạ cấu hình cho hàng này. 
4. Khởi tạo dp cho hàng 0 bằng cách đặt dp[0][mask] thành số bit đã đặt trong mặt nạ cho tất cả các mặt nạ hợp lệ. 
5. Đối với mỗi hàng tiếp theo, hãy chuyển từ mọi mặt nạ hợp lệ trước đó sang mọi mặt nạ hiện tại hợp lệ, nhưng chỉ khi hai mặt nạ không chồng lên nhau theo chiều dọc. Điều này có nghĩa là không có cột nào có số 1 trong cả hai mặt nạ, vì điều đó sẽ xếp các miêu nữ vào các hàng liền kề trong cùng một cột. 
6. Cập nhật dp[row][cur_mask] là mức tối đa trên tất cả các mặt nạ tương thích trước đó của dp[row-1][prev_mask] cộng với số lượng của cur_mask. 
7. Câu trả lời là giá trị lớn nhất trên tất cả dp[last_row][mask]. 

Lý do chúng ta có thể thực hiện việc này một cách hiệu quả là vì mỗi hàng chỉ phụ thuộc vào hàng trước đó, do đó trạng thái thu gọn về không gian 2^m có thể quản lý thay vì theo cấp số nhân trên tất cả các ô. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt đầy đủ lịch sử liên quan duy nhất cần thiết cho các quyết định trong tương lai, đó là cấu hình đã chọn của hàng trước đó. Bất kỳ hàng nào trước đó chỉ ảnh hưởng đến tương lai thông qua đóng góp của chúng vào tổng số, đã được lưu trong giá trị DP. Việc kiểm tra tính tương thích đảm bảo rằng không xảy ra vi phạm lân cận theo chiều dọc hoặc chiều ngang, do đó mọi chuyển đổi đều duy trì tính hợp lệ. Vì mọi vị trí chung hợp lệ đều tương ứng với chính xác một chuỗi mặt nạ hàng và tất cả các chuỗi như vậy đều được xem xét nên đảm bảo tìm thấy mức tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid_masks(row_str, m):
    masks = []
    for mask in range(1 << m):
        ok = True
        for j in range(m):
            if (mask >> j) & 1:
                if row_str[j] == 'N':
                    ok = False
                    break
                if j > 0 and (mask >> (j - 1)) & 1:
                    ok = False
                    break
        if ok:
            cnt = bin(mask).count("1")
            masks.append((mask, cnt))
    return masks

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    row_masks = [valid_masks(grid[i], m) for i in range(n)]

    prev = {0: 0}

    for i in range(n):
        curr = {}
        for cmask, ccnt in row_masks[i]:
            best = 0
            for pmask, pval in prev.items():
                if pmask & cmask == 0:
                    best = max(best, pval + ccnt)
            curr[cmask] = max(curr.get(cmask, 0), best)
        prev = curr

    print(max(prev.values()))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng, cho mỗi hàng, tất cả các cấu hình tôn trọng sự kề cận theo chiều ngang và tránh các ô bị cấm. Từ điển DP`prev`lưu trữ giá trị tốt nhất có thể đạt được cho mỗi mặt nạ của hàng trước đó. 

Kiểm tra chuyển tiếp`pmask & cmask == 0`, thực thi các ràng buộc kề dọc theo từng cột. Nếu cả hai hàng đặt một miêu nữ trong cùng một cột, chúng sẽ nằm cạnh nhau theo chiều dọc, điều này bị cấm. 

DP dựa trên từ điển tránh hoàn toàn việc lặp lại các trạng thái không hợp lệ, điều này giúp việc triển khai trở nên đơn giản với các ràng buộc nhỏ. Vì m ≤ 8 nên số lượng mặt nạ tối đa là 256 mỗi hàng, rất dễ quản lý. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ 3 × 3:```
YYY
YNY
YYY
```Chúng tôi theo dõi DP mỗi hàng. 

### Hàng 0 

| mặt nạ | hợp lệ | đếm | dp | 
| --- | --- | --- | --- | 
| 000 | vâng | 0 | 0 | 
| 001 | vâng | 1 | 1 | 
| 010 | vâng | 1 | 1 | 
| 100 | vâng | 1 | 1 | 
| 101 | vâng | 2 | 2 | 
| 110 | vâng | 2 | 2 | 

### Hàng 1 (hàng giữa bị chặn ở giữa) 

Mặt nạ hợp lệ loại trừ bất kỳ mặt nạ nào có bit trung tâm được đặt. 

| mặt nạ | hợp lệ | đếm | 
| --- | --- | --- | 
| 000 | vâng | 0 | 
| 001 | vâng | 1 | 
| 010 | không | - | 
| 100 | vâng | 1 | 
| 101 | vâng | 2 | 
| 110 | không | - | 

Transition kết hợp các mặt nạ tương thích. Ví dụ: mặt nạ 101 ở hàng 1 không thể ghép nối với hàng 0 mặt nạ 101 do chồng chéo cột, nhưng có thể ghép nối với 010 hoặc 001 tùy theo ràng buộc. 

DP tiến hóa bằng cách lấy số tiền tương thích tối đa. 

### Hàng 2 

Tương tự như hàng 0, nhưng quá trình chuyển đổi chỉ phụ thuộc vào kết quả của hàng 1. 

Dấu vết này cho thấy các quyết định được bản địa hóa trên mỗi hàng và xung đột chỉ phát sinh thông qua sự chồng chéo các cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 4^m) | Đối với mỗi hàng, chúng tôi thử tất cả các mặt nạ hợp lệ với tất cả các mặt nạ trước đó, mỗi mặt nạ có kích thước tối đa 2^m | 
| Không gian | O(2^m) | Chúng tôi chỉ lưu trữ DP cho các trạng thái hàng trước đó | 

Vì m 8, 4^m 65536 và n 8, tổng công việc đủ nhỏ cho giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def valid_masks(row_str, m):
        masks = []
        for mask in range(1 << m):
            ok = True
            for j in range(m):
                if (mask >> j) & 1:
                    if row_str[j] == 'N':
                        ok = False
                        break
                    if j > 0 and (mask >> (j - 1)) & 1:
                        ok = False
                        break
            if ok:
                cnt = bin(mask).count("1")
                masks.append((mask, cnt))
        return masks

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    row_masks = [valid_masks(grid[i], m) for i in range(n)]

    prev = {0: 0}
    for i in range(n):
        curr = {}
        for cmask, ccnt in row_masks[i]:
            best = 0
            for pmask, pval in prev.items():
                if pmask & cmask == 0:
                    best = max(best, pval + ccnt)
            curr[cmask] = max(curr.get(cmask, 0), best)
        prev = curr

    return str(max(prev.values()))

# provided sample (structure inferred)
assert run("4 4\nYYYY\nYYNN\nNYNY\nYYYY\n") == "7"

# all blocked
assert run("2 2\nNN\nNN\n") == "0"

# single row all valid
assert run("1 5\nYYYYY\n") == "3"

# checkerboard free placement
assert run("3 3\nYYY\nYYY\nYYY\n") == "5"

# sparse diagonal
assert run("3 3\nYNN\nNYN\nNNY\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả lưới N | 0 | tính khả thi trống rỗng | 
| hàng đơn | max độc lập được đặt thẳng hàng | ràng buộc ngang | 
| lưới đầy đủ | đóng gói dày đặc với xung đột | DP chính xác | 
| đường chéo | không có tương tác lân cận | tách dọc/ngang | 

## Vỏ cạnh 

Lưới bị chặn hoàn toàn được xử lý rõ ràng vì mỗi hàng chỉ có mặt nạ 0 hợp lệ. DP không bao giờ tăng quá 0 và trả về 0. 

Lưới một hàng giảm xuống việc chọn các ô không liền kề trong một dòng. Việc tạo mặt nạ hàng thực thi không có bit liên tiếp, do đó DP thu gọn chính xác thành tập hợp độc lập tối đa 1D. 

Một mô hình đường chéo như```
YNN
NYN
NNY
```đảm bảo rằng sự liền kề theo chiều dọc không bao giờ kích hoạt. Mỗi hàng độc lập cho phép một vị trí duy nhất và vì sự chồng chéo cột không bao giờ xảy ra nên DP tích lũy cả ba vị trí.
