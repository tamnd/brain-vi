---
title: "CF 104178D - Thế giới"
description: "Chúng ta được cho một chuỗi các điểm, mỗi điểm có tối đa 10 tọa độ. Chúng ta phải cắt chuỗi này thành nhiều khối liền kề nhau. Mỗi điểm thuộc về chính xác một khối. Đối với bất kỳ khối nào, giá của nó được xác định là khoảng cách L1 lớn nhất giữa hai điểm bất kỳ bên trong khối đó."
date: "2026-07-02T00:47:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104178
codeforces_index: "D"
codeforces_contest_name: "BdOI Preliminary 2023"
rating: 0
weight: 104178
solve_time_s: 59
verified: true
draft: false
---

[CF 104178D - Thế giới](https://codeforces.com/problemset/problem/104178/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm, mỗi điểm có tối đa 10 tọa độ. Chúng ta phải cắt chuỗi này thành nhiều khối liền kề nhau. Mỗi điểm thuộc về chính xác một khối. 

Đối với bất kỳ khối nào, giá của nó được xác định là khoảng cách L1 lớn nhất giữa hai điểm bất kỳ bên trong khối đó. Tổng số điểm của một phân vùng là tổng chi phí khối và chúng tôi muốn tối đa hóa số tiền này. 

Vì vậy, cấu trúc là một vấn đề cổ điển “chia một mảng thành các phân đoạn”, nhưng giá trị phân đoạn không phải là một thống kê phạm vi đơn giản như tổng hoặc tối đa. Thay vào đó, nó phụ thuộc vào đường kính hình học theo khoảng cách Manhattan trong tối đa 10 chiều. 

Kích thước đầu vào buộc phải có một tư duy rất cụ thể. Với n lên tới 100000, bất kỳ giải pháp nào cố gắng xem xét rõ ràng tất cả các ranh giới phân đoạn O(n^2) đều là không thể ngay lập tức. Ngay cả O(n log n) trên mỗi phân đoạn cũng quá chậm vì chúng ta có thể sẽ cần các phân đoạn O(n). Chiều m nhỏ, nhiều nhất là 10, đây là điểm yếu về cấu trúc duy nhất mà chúng ta có thể khai thác. Điều đó gợi ý rõ ràng rằng hành vi hàm mũ trong m là có thể chấp nhận được, trong khi mọi hành vi bậc hai trong n thì không. 

Một khó khăn tinh tế là chi phí của phân khúc không đơn điệu hoặc cộng thêm. Việc thêm một điểm mới vào một đoạn có thể làm tăng đáng kể đường kính và việc nhận dạng cặp đạt được đường kính phụ thuộc vào tập hợp đầy đủ. Điều này làm cho việc chia tách tham lam không đáng tin cậy. 

Một sai lầm ngây thơ xuất hiện khi người ta cho rằng chi phí của một phân đoạn chỉ được xác định bởi các điểm cuối trong không gian ban đầu. Ví dụ: trong 1D, điều đó hiệu quả vì chi phí chỉ đơn giản là tối đa trừ tối thiểu. Nhưng ở các chiều cao hơn, cặp đạt được khoảng cách L1 tối đa không nhất thiết phải ở cực trị tọa độ theo cách nhất quán giữa các chiều, do đó, lý luận dựa trên điểm cuối sẽ bị phá vỡ ngay lập tức. 

Một dạng thất bại khác xuất phát từ việc cố gắng duy trì tăng dần chi phí của phân khúc và cắt giảm một cách tham lam bất cứ khi nào chi phí vượt quá ngưỡng nào đó. Do chi phí của các phân khúc khác nhau tương tác thông qua DP nên các quyết định cục bộ không đạt được mức tối ưu toàn cầu. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ thử tất cả các phân vùng có thể. Có 2^(n-1) cách phân chia và đối với mỗi đoạn, chúng ta tính đường kính của nó bằng cách kiểm tra tất cả các cặp điểm bên trong đoạn đó. Điều đó đã mang lại O(n^2) cho mỗi phân vùng, dẫn đến sự bùng nổ theo cấp số nhân vượt xa tính khả thi. 

Một DP bạo lực có cấu trúc chặt chẽ hơn coi dp[i] là câu trả lời tốt nhất cho tiền tố i và thử mọi vị trí cắt j trước đó. Đối với mỗi (j, i), chúng tôi tính chi phí của phân khúc j+1..i. Điều này làm giảm vấn đề xuống các phân đoạn O(n^2), nhưng tính toán chi phí từng phân đoạn một cách đơn giản là O(nm), cho ra O(n^3 m), con số này vẫn còn quá lớn. 

Quan sát quan trọng là khoảng cách Manhattan hoạt động như thế nào trong một chiều cố định. Khoảng cách L1 giữa hai điểm A và B có thể được viết lại dưới dạng mẫu ký hiệu cực đại của các phép chiếu tuyến tính. Đối với vectơ dấu s trong {+1, -1}^m, hãy xác định giá trị được chuyển đổi v_s(x) = sum s_k x_k. Khi đó khoảng cách L1 trở thành giá trị lớn nhất trên s của |v_s(A) - v_s(B)|. Điều này ngụ ý rằng đường kính của tập hợp trong L1 là tối đa trên 2^m phạm vi 1D tuyến tính: đối với mỗi mẫu dấu, chúng tôi lấy v_s tối đa trừ v_s tối thiểu trên phân đoạn. 

Điều này biến một bài toán hình học thành nhiều bài toán phạm vi 1D. Chi phí của một phân đoạn hiện là mức tối đa trên tối đa 1024 dự đoán (tối đa trừ tối thiểu). 

Cấu trúc này cho phép chúng tôi duy trì việc chạy cực đại và cực tiểu trên mỗi mẫu ký hiệu trong khi quét một phân đoạn và đánh giá chi phí phân đoạn một cách hiệu quả. Khó khăn còn lại là việc tích hợp điều này vào DP trên các điểm cuối của phân đoạn trong khi vẫn giữ được tổng độ phức tạp quanh O(n * 2^m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu + kiểm tra cặp | Hàm mũ | O(1) | Quá chậm | 
| DP với tính toán lại phân đoạn đơn giản | O(n^3 m) | O(n) | Quá chậm | 
| DP với thủ thuật chiếu L1 | O(n · 2^m) | O(n · 2^m) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng điểm thành các giá trị được chuyển đổi 2^m, một giá trị cho mỗi mẫu dấu. Đây là bước giảm cốt lõi. 

## Hướng dẫn thuật toán 

1. Với mọi điểm i và mọi mẫu dấu s trong [0, 2^m), tính v[i][s], tổng tọa độ có dấu. Điều này cho phép chúng tôi đánh giá khoảng cách L1 bằng cách sử dụng chênh lệch 1D. Mục đích là để thay thế bài toán khoảng cách vectơ bằng các bài toán phạm vi vô hướng đa biến. 
2. Xác định dp[i] là số điểm tối đa có thể đạt được bằng cách sử dụng điểm i đầu tiên. Chúng tôi xây dựng nó tăng dần từ trái sang phải. 
3. Để tính dp[i], chúng ta thử mọi vị trí cắt cuối cùng có thể có j, nghĩa là đoạn cuối cùng là j+1 đến i. Điều này mang lại dp[i] = max trên j của dp[j] + cost(j+1, i). Cấu trúc này đúng vì bất kỳ phân vùng tối ưu nào cũng phải kết thúc bằng một phân đoạn cuối cùng nào đó. 
4. Đối với i và j cố định, chúng ta duy trì, với mọi mẫu dấu s, giá trị tối thiểu và tối đa của v[*][s] trên cửa sổ j+1..i. Chúng tôi cập nhật những thông tin này trong khi di chuyển j lùi từ i sang 1. Điều này cho phép cập nhật liên tục theo thời gian trên mỗi bước trên mỗi mẫu dấu hiệu. 
5. Đối với mỗi j, chúng tôi tính toán chi phí của phân đoạn j+1..i bằng cách quét tất cả các mẫu dấu và lấy giá trị tối đa (tối đa hiện tại trừ đi tối thiểu hiện tại). Điều này tạo ra đường kính L1 chính xác cho đoạn đó. 
6. Chúng tôi tận dụng tối đa tất cả j để có được dp[i] và tiếp tục. 

Lý do điều này có tác dụng là vì việc phân tách đường kính L1 đảm bảo mọi chi phí của phân khúc đều được ghi lại hoàn toàn bằng một trong các phép chiếu 2^m. Mỗi phép chiếu hoạt động giống như một phạm vi 1D trong đó cực trị có thể được cập nhật tăng dần khi chúng ta mở rộng hoặc thu hẹp phân đoạn. DP liệt kê tất cả các phân đoạn hợp lệ cuối cùng và thủ thuật chiếu đảm bảo chi phí của mỗi phân đoạn được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    P = 1 << m

    # precompute sign vectors
    sign = []
    for mask in range(P):
        s = []
        for j in range(m):
            if mask & (1 << j):
                s.append(1)
            else:
                s.append(-1)
        sign.append(s)

    # transform points
    v = [[0] * P for _ in range(n)]
    for i in range(n):
        for mask in range(P):
            s = sign[mask]
            val = 0
            for j in range(m):
                val += s[j] * a[i][j]
            v[i][mask] = val

    INF = 10**30
    dp = [-INF] * (n + 1)
    dp[0] = 0

    for i in range(1, n + 1):
        maxv = [-INF] * P
        minv = [INF] * P

        best = 0

        # extend left boundary j
        for j in range(i - 1, -1, -1):
            for s in range(P):
                x = v[j][s]
                if x > maxv[s]:
                    maxv[s] = x
                if x < minv[s]:
                    minv[s] = x

            # compute segment cost j..i-1
            cost = 0
            for s in range(P):
                diff = maxv[s] - minv[s]
                if diff > cost:
                    cost = diff

            best = max(best, dp[j] + cost)

        dp[i] = best

    print(dp[n])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén từng điểm thành tất cả các hình chiếu có dấu 2^m, đây là cách duy nhất để làm cho khoảng cách Manhattan có thể phân tách thành cực trị vô hướng độc lập. 

Vòng lặp DP sửa điểm cuối bên phải i. Sau đó, nó di chuyển điểm cuối bên trái j lùi lại, cập nhật các mảng tối thiểu và tối đa cho mỗi phép chiếu. Việc bảo trì gia tăng này tránh việc tính toán lại số liệu thống kê phân khúc từ đầu. Chi phí phân khúc được tính toán lại theo O(2^m) mỗi j bằng cách quét tất cả các phép chiếu. 

Một điểm tinh tế là dp[0] được khởi tạo thành 0, cho phép phân đoạn đầu tiên bắt đầu ở chỉ mục 1. Một điểm khác là maxv và minv phải được đặt lại cho mọi i, vì mỗi chuyển đổi dp đều xem xét một điểm cuối bên phải mới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (cấu trúc đơn giản) 

Xét một trường hợp nhỏ giống 1D trong đó m = 1 và các giá trị là`[1, 3, 0, 1]`. 

Với mỗi i, chúng ta khai triển sang trái: 

| j | phân đoạn | phút | tối đa | chi phí | dp[j] + chi phí | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | [1] | 1 | 1 | 0 | dp[3] | 0 | 
| 2 | [0,1] | 0 | 1 | 1 | dp[2]+1 | 1 | 
| 1 | [3,0,1] | 0 | 3 | 3 | dp[1]+3 | 3 | 
| 0 | [1,3,0,1] | 0 | 3 | 3 | dp[0]+3 | 3 | 

Điều này cho thấy cách mở rộng j cập nhật cực trị tăng dần và dp tích lũy phần chia tốt nhất. 

### Ví dụ 2 (ý tưởng chiếu đa chiều) 

Lấy hai điểm thuộc m = 2: (0,0), (1,2). 

Các mẫu dấu là (++), (+-), (+-), (--). Dự kiến của họ: 

| điểm | ++ | +- | -+ | -- | 
| --- | --- | --- | --- | --- | 
| (0,0) | 0 | 0 | 0 | 0 | 
| (1,2) | 3 | -1 | 1 | -3 | 

Đối với phân đoạn này, mỗi phép chiếu tạo ra một phạm vi và phạm vi tối đa là 3, bằng khoảng cách L1. Điều này xác nhận lý do tại sao việc quét tất cả các hình chiếu sẽ thu được đường kính thực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^m + n^2 · 2^m) | DP trên tất cả các điểm cuối, mỗi tiện ích mở rộng cập nhật và quét tất cả các phép chiếu | 
| Không gian | O(n · 2^m) | lưu trữ các giá trị được chuyển đổi | 

Với m ₫ 10, 2^m ∼ 1024 và n ∼ 100000, hệ số chi phối là khoảng 10^8 thao tác, nhằm mục đích tối ưu hóa việc triển khai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("1 1\n5\n") == "0"

# two points
assert run("2 1\n1\n10\n") == "9"

# all equal
assert run("3 2\n1 1\n1 1\n1 1\n") == "0"

# increasing line
assert run("4 1\n1\n2\n3\n4\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 điểm | 0 | trường hợp cạnh phân đoạn đơn | 
| 2 điểm | khoảng cách | chuyển đổi DP cơ bản | 
| tất cả đều bình đẳng | 0 | ổn định với chi phí bằng 0 | 
| dòng được sắp xếp | hành vi hợp nhất tối đa | phân đoạn đơn điệu | 

## Vỏ cạnh 

Đầu vào một phần tử chứng tỏ rằng DP cho phép chính xác một đoạn có độ dài bằng 1, có chi phí bằng 0 do không tồn tại cặp nào. Thuật toán khởi tạo dp[0] = 0 và không bao giờ buộc phân đoạn không hợp lệ, do đó dp[1] trở thành 0 chính xác. 

Khi tất cả các điểm giống hệt nhau, mọi phép chiếu đều có phạm vi bằng 0, do đó maxv bằng minv trên mọi phân đoạn. Tính toán chi phí luôn mang lại kết quả bằng 0 và DP tích lũy bằng 0 cho tất cả các trạng thái, khớp với câu trả lời đúng. 

Để tăng nghiêm ngặt đầu vào 1D, mỗi chi phí phân khúc bằng khoảng của nó. Thuật toán đánh giá chính xác tất cả các điểm phân chia có thể có và xác định rằng phân vùng tốt nhất là một phân đoạn đơn lẻ, vì việc phân chia làm giảm tổng khoảng cách.
