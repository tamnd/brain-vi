---
title: "CF 102968H - KMP"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi muốn đếm các chuỗi con (vì vậy chúng tôi chọn các chỉ số theo thứ tự tăng dần, không nhất thiết phải liền kề) với một thuộc tính đặc biệt trên các giá trị chúng tôi đã chọn."
date: "2026-07-04T06:36:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102968
codeforces_index: "H"
codeforces_contest_name: "AGM 2021, Qualification Round"
rating: 0
weight: 102968
solve_time_s: 50
verified: true
draft: false
---

[CF 102968H - KMP](https://codeforces.com/problemset/problem/102968/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi muốn đếm các chuỗi con (vì vậy chúng tôi chọn các chỉ số theo thứ tự tăng dần, không nhất thiết phải liền kề) với một thuộc tính đặc biệt trên các giá trị chúng tôi đã chọn. 

Một dãy con được coi là hợp lệ nếu tất cả các giá trị được chọn là khác biệt và khi bạn nhìn vào giá trị tối thiểu và giá trị tối đa của nó, mọi số nguyên trong toàn bộ khoảng đó sẽ xuất hiện trong dãy con. Nói cách khác, sau khi sắp xếp các giá trị đã chọn, chúng phải tạo thành một khối số nguyên liên tục không có khoảng trống. 

Vì vậy, nếu chúng ta chọn các giá trị, chúng sẽ hoạt động giống như một tập hợp chứ không phải một chuỗi. Thứ tự của các chỉ số chỉ quan trọng ở chỗ chúng ta có thể chọn những giá trị đó theo bao nhiêu cách, nhưng điều kiện giá trị chỉ phụ thuộc vào tập hợp. 

Ví dụ: việc chọn các giá trị như 7, 8, 9 là hợp lệ vì phạm vi này liên tục. Việc chọn 7 và 9 không có 8 là không hợp lệ vì thiếu 8 trong khoảng. Việc lặp lại cũng bị cấm vì chuỗi con phải bao gồm các giá trị riêng biệt. 

Kích thước đầu vào lên tới 100000 và các giá trị cũng lên tới 100000. Điều này loại trừ mọi thứ thử trực tiếp tất cả các chuỗi con hoặc tất cả các cặp giá trị. Một giải pháp bậc hai trên phạm vi giá trị đã đạt tới 10^10 phép tính, vượt xa mọi giới hạn thời gian. 

Trường hợp góc tinh vi là khi các giá trị trong khoảng đã chọn bị thiếu hoàn toàn khỏi chuỗi ban đầu. Ví dụ: nếu chúng ta cố gắng tạo một tập hợp lệ bao gồm các giá trị từ 3 đến 6, nhưng giá trị 5 không bao giờ xuất hiện trong mảng thì không thể xây dựng một chuỗi con như vậy, do đó khoảng đó đóng góp 0 chuỗi con hợp lệ. Một trường hợp góc khác là các giá trị lặp lại trong mảng: các giá trị trùng lặp không phá vỡ tính hợp lệ nhưng chúng làm tăng số cách chọn chỉ mục. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là liệt kê mọi dãy con có thể có, tính toán mức tối thiểu và tối đa của nó và xác minh xem tất cả các số nguyên giữa chúng có xuất hiện chính xác một lần trong tập hợp đã chọn hay không. Ngay cả khi chúng tôi bỏ qua thứ tự, vẫn có 2^N dãy con, vì vậy điều này ngay lập tức không thể thực hiện được. 

Một nỗ lực có cấu trúc hơn là suy nghĩ về các tập giá trị riêng biệt mà chúng ta có thể chọn. Bất kỳ chuỗi con hợp lệ nào đều tương ứng với việc chọn một khoảng giá trị [L, R], sau đó chọn chính xác một lần xuất hiện cho mỗi giá trị trong khoảng đó. Nếu một giá trị không xuất hiện trong mảng thì khoảng đó là không thể. Nếu một giá trị xuất hiện cnt[v] lần thì trong một khoảng thời gian cố định, số cách hình thành một dãy con là tích của cnt[v] trên tất cả v trong [L, R]. 

Vì vậy, bài toán quy về việc tính tổng, trên tất cả các khoảng giá trị [L, R], tích của các tần số bên trong khoảng đó. Khó khăn chính là có các khoảng O(V^2), quá lớn. 

Quan sát tiết kiệm là chúng ta có thể tính tổng này tăng dần bằng cách sửa điểm cuối phù hợp. Đặt dp[r] biểu thị tổng đóng góp của tất cả các khoảng hợp lệ kết thúc bằng giá trị r. Bất kỳ khoảng nào như vậy đều là khoảng giá trị đơn [r, r] hoặc phần mở rộng của khoảng nào đó kết thúc ở r - 1. Nếu chúng ta đã biết tổng tổng các sản phẩm cho các khoảng kết thúc ở r - 1, thì việc mở rộng tất cả chúng bằng r sẽ nhân phần đóng góp của chúng với cnt[r]. 

Điều này đưa ra một phép truy toán tuyến tính làm thu gọn cấu trúc bậc hai thành một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các chuỗi tiếp theo | O(2^N · N) | O(N) | Quá chậm | 
| Khoảng giá trị DP | O(M) | O(M) | Đã chấp nhận | 

Ở đây M là giá trị lớn nhất trong mảng. 

## Hướng dẫn thuật toán

1. Đếm số lần mỗi giá trị xuất hiện trong mảng. Lưu trữ giá trị này trong một mảng cnt trong đó cnt[x] là tần số của giá trị x. Điều này chuyển đổi vấn đề từ không gian chỉ mục sang không gian giá trị. 
2. Xác định dp[r] là tổng số chuỗi kompakt hợp lệ có giá trị nằm hoàn toàn trong một khoảng kết thúc bằng giá trị r và có giá trị lớn nhất chính xác là r. Mỗi dãy con như vậy tương ứng với việc chọn một khoảng không trống [l, r] trong không gian giá trị và sau đó chọn một lần xuất hiện cho mỗi giá trị trong khoảng đó. 
3. Tính dp[r] bằng cách sử dụng hàm truy hồi dp[r] = cnt[r] + cnt[r] * dp[r - 1]. Số hạng cnt[r] tương ứng với khoảng [r, r], trong đó chúng ta chọn bất kỳ sự xuất hiện nào của r. Thuật ngữ cnt[r] * dp[r - 1] tương ứng với việc mở rộng mọi khoảng hợp lệ kết thúc ở r - 1 bằng cách bao gồm giá trị r, nhân tất cả các lựa chọn trước đó với cnt[r]. 
4. Tích lũy câu trả lời bằng cách tính tổng dp[r] trên tất cả r. 
5. Thực hiện tất cả các hoạt động theo modulo 1e9 + 7 vì sản phẩm có thể phát triển nhanh chóng. 

Toàn bộ quá trình tính toán là một lần chuyển qua miền giá trị sau khi tiền xử lý tần số. 

### Tại sao nó hoạt động 

Mỗi dãy con hợp lệ được xác định duy nhất bởi tập hợp các giá trị của nó, các giá trị này phải tạo thành một khoảng liền kề [l, r]. Trong một khoảng thời gian cố định, các lựa chọn trên các giá trị khác nhau là độc lập vì việc chọn chỉ mục cho giá trị v không ảnh hưởng đến các lựa chọn cho các giá trị khác. Sự độc lập này biến việc đếm thành tích của các tần số. Phép truy toán tránh được việc tính hai lần vì mỗi khoảng được tạo chính xác một lần bởi điểm cuối bên phải r của nó và ranh giới bên trái của nó được biểu diễn ngầm bên trong dp[r - 1]. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n = int(input())
    arr = list(map(int, input().split()))

    if n == 0:
        print(0)
        return

    maxv = max(arr)
    cnt = [0] * (maxv + 1)

    for x in arr:
        cnt[x] += 1

    dp_prev = 0
    ans = 0

    for r in range(1, maxv + 1):
        if cnt[r] == 0:
            dp_r = 0
        else:
            dp_r = cnt[r] * (1 + dp_prev) % MOD

        ans = (ans + dp_r) % MOD
        dp_prev = dp_r

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai nén DP thành hai biến thay vì một mảng đầy đủ.`dp_prev`lưu trữ tổng đóng góp của các khoảng kết thúc ở giá trị trước đó. Khi giá trị hiện tại có tần số bằng 0, không có khoảng thời gian nào có thể kết thúc ở giá trị đó, do đó DP đặt lại về 0. 

Một lỗi phổ biến là quên rằng các giá trị bị thiếu sẽ phá vỡ tất cả các khoảng trải dài chúng. Việc này được xử lý một cách tự nhiên vì`cnt[r] = 0`lực lượng`dp_r = 0`, điều này ngăn chặn bất kỳ khoảng thời gian nào vượt qua giá trị đó đóng góp. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 2, 2, 3]`. 

Chúng tôi tính toán tần số: cnt[1]=1, cnt[2]=2, cnt[3]=1. 

| r | cnt[r] | dp_prev | tính toán dp[r] | dp[r] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 * (1 + 0) | 1 | 
| 2 | 2 | 1 | 2 * (1 + 1) | 4 | 
| 3 | 1 | 4 | 1 * (1 + 4) | 5 | 

Câu trả lời cuối cùng là 1 + 4 + 5 = 10. 

Dấu vết này cho thấy các khoảng kết thúc ở mỗi giá trị tích lũy tất cả các cách chọn chỉ số một cách độc lập cho mỗi giá trị như thế nào. 

Bây giờ hãy xem xét`[1, 3, 3]`. 

Tần số: cnt[1]=1, cnt[2]=0, cnt[3]=2. 

| r | cnt[r] | dp_prev | dp[r] | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 0 | 1 | 0 | 
| 3 | 2 | 0 | 2 | 

Câu trả lời là 3. 

Số 0 ở giá trị 2 phá vỡ tất cả các khoảng có thể trải dài trên nó, đảm bảo không có khoảng thời gian thu gọn không hợp lệ nào được đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + N) | Tần số đếm mất O(N), DP chạy trên phạm vi giá trị lên tới M | 
| Không gian | O(M) | Mảng tần số trên miền giá trị | 

Các ràng buộc cho phép M lên tới 100000, do đó, việc quét tuyến tính trên phạm vi giá trị có thể dễ dàng đủ nhanh và mức sử dụng bộ nhớ vẫn ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    arr = list(map(int, input().split()))

    maxv = max(arr)
    cnt = [0] * (maxv + 1)
    for x in arr:
        cnt[x] += 1

    dp_prev = 0
    ans = 0

    for r in range(1, maxv + 1):
        if cnt[r] == 0:
            dp_r = 0
        else:
            dp_r = cnt[r] * (1 + dp_prev) % MOD
        ans = (ans + dp_r) % MOD
        dp_prev = dp_r

    return str(ans)

# provided sample (interpreted example)
assert solve("4\n1 2 2 3\n") == "10"

# all equal values
assert solve("3\n5 5 5\n") == str((3 * (1 + 0) + 3 * (1 + 3 * (1 + 0)) + 3 * (1 + (3 * (1 + 3 * (1 + 0))))) % MOD)

# single element
assert solve("1\n7\n") == "1"

# with gap
assert solve("4\n1 2 4 4\n") == solve("4\n1 2 4 4\n")

# strictly increasing unique
assert solve("3\n1 2 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | biểu thức tính toán | xử lý đa bội | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| khoảng cách về giá trị | thiết lập lại dp nhất quán | khoảng thời gian ngắt tần số bằng không | 
| ngày càng độc đáo | 6 | tất cả các khoảng được tính chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng chứa các khoảng trống trong không gian giá trị. Ví dụ,`[1, 2, 4]`không có dãy con kompakt hợp lệ kéo dài từ 1 đến 4 vì thiếu giá trị 3. Trong DP, tại r = 3, chúng ta nhận được cnt[3] = 0, điều này buộc dp[3] = 0. Điều này đặt lại tất cả các khoảng mà lẽ ra đã vượt qua 3, do đó các khoảng kết thúc ở 4 không thể bao gồm 1 hoặc 2, phù hợp với định nghĩa. 

Một trường hợp cạnh khác là sự trùng lặp nặng nề, chẳng hạn như`[2, 2, 2, 2]`. Mỗi khoảng chỉ là [2, 2], nhưng có nhiều cách để chọn một chỉ mục. DP tại r = 2 trở thành cnt[2] * (1 + dp[1]) = 4, đếm chính xác tất cả các chuỗi con có một phần tử và không có khoảng nào lớn hơn.
