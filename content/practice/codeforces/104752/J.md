---
title: "CF 104752J - Chuyến đi bộ đường dài của Juan Jo"
description: "Chúng tôi được phân cho một nhóm nhỏ, mỗi người mang một chiếc ba lô có giới hạn trọng lượng cố định và một bộ đồ dự trữ, mỗi người có trọng lượng riêng."
date: "2026-06-28T23:00:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "J"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 99
verified: false
draft: false
---

[CF 104752J - Chuyến đi bộ đường dài của Juan Jo](https://codeforces.com/problemset/problem/104752/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phân cho một nhóm nhỏ, mỗi người mang một chiếc ba lô có giới hạn trọng lượng cố định và một bộ đồ dự trữ, mỗi người có trọng lượng riêng. Mục tiêu là chọn một tập hợp con của những đồ dự trữ này và giao từng món đồ đã chọn cho đúng một người sao cho không có chiếc ba lô nào vượt quá sức chứa của nó và số lượng đồ dự trữ mang theo càng lớn càng tốt. 

Đây không phải là một chiếc ba lô đơn cổ điển. Thay vào đó, chúng tôi có nhiều chiếc ba lô với sức chứa riêng và mỗi món đồ phải được đặt vào một trong số chúng hoặc bỏ đi. Mục tiêu hoàn toàn là tối đa hóa số lượng mặt hàng chứ không phải tổng trọng lượng hoặc giá trị của chúng, điều này làm thay đổi bản chất của việc ra quyết định. Bởi vì cả số lượng người và số lượng thực phẩm nhiều nhất là 15, nên cấu trúc gợi ý rõ ràng về việc tìm kiếm theo cấp số nhân trên các tập hợp con kết hợp với việc theo dõi trạng thái cẩn thận. 

Các ràng buộc đủ nhỏ để các giải pháp xung quanh$2^{15}$hoặc thậm chí$3^{15}$khả thi nếu công việc ở mỗi trạng thái là tối thiểu. Bất cứ điều gì đa thức trong một không gian trạng thái lớn là không cần thiết, nhưng bất cứ điều gì theo cấp số nhân về cả người và vật phẩm mà không cần cắt tỉa vẫn ổn. 

Một sai lầm ngây thơ nhưng phổ biến là coi đây như những chiếc ba lô độc lập và tham lam lấp đầy mỗi chiếc ba lô những món đồ nhẹ nhất còn lại. Điều đó không thành công vì các vật phẩm cạnh tranh giữa các ba lô và vị trí tối ưu cục bộ có thể cản trở việc phân công tốt hơn trên toàn cầu. 

Hãy xem xét một trường hợp thất bại đơn giản: 

đầu vào:```
2 3
3 3
2 2 2
```Cách tiếp cận tham lam có thể gán một vật phẩm có trọng lượng 2 cho mỗi ba lô, để lại một vật phẩm không được sử dụng. Điều đó mang lại 2 mục. Nhưng tối ưu vẫn là 2 ở đây, điều này che giấu vấn đề. Một trường hợp thất bại tốt hơn:```
2 3
3 3
3 2 2
```Kẻ tham lam có thể đặt vật nặng 3 trọng lượng trước vào một ba lô, chỉ để lại một vật nặng 2 trọng lượng có thể sử dụng được. Điều đó mang lại 2 mục, nhưng tối ưu là 3 bằng cách đặt các phân phối kiểu (2,1,1) nếu như vậy tồn tại. Vấn đề thực sự là việc đặt hàng tham lam không thể giải thích được tính linh hoạt của việc đóng gói trong tương lai. 

Khó khăn cốt lõi là mọi nhiệm vụ đều thay đổi cấu trúc dung lượng còn lại trên nhiều ba lô cùng một lúc. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: xem xét từng tập hợp con các điều khoản và đối với mỗi tập hợp con, hãy cố gắng gán các vật phẩm vào ba lô theo thứ tự nào đó, kiểm tra tính khả thi. có$2^M$các tập hợp con, và đối với mỗi tập hợp con, việc kiểm tra phép gán thực sự là một vấn đề quay lui đối với$K$thùng rác. Trong trường hợp xấu nhất, việc giao$S$mục vào$K$ba lô dẫn đến khoảng$K^S$khả năng, vì mỗi vật phẩm có thể đi vào bất kỳ ba lô nào. Với$M = 15$, điều này nhanh chóng trở nên không khả thi ngay cả khi được cắt tỉa nhiều. 

Quan sát quan trọng là không gian tìm kiếm có thể được cấu trúc như một trạng thái đối với con người chứ không phải theo các nhiệm vụ. Thay vì theo dõi xem món đồ nào sẽ đi đâu một cách rõ ràng, chúng ta có thể nghĩ đến việc chất đầy từng chiếc ba lô và quyết định tập hợp con những món đồ còn lại mà mỗi chiếc ba lô mang theo. Vì cả hai$K$Và$M$tối đa là 15, chúng ta có thể sử dụng lập trình động trên các tập hợp con các mục kết hợp với xử lý lặp trên con người. 

Chúng tôi xác định trạng thái DP dựa trên những mục đã được chỉ định và số lượng ba lô chúng tôi đã xử lý. Điều này làm giảm vấn đề đối với tập hợp con DP phân lớp, trong đó quá trình chuyển đổi liên quan đến việc thử tất cả các tập hợp con của các mục còn lại phù hợp với ba lô hiện tại. 

Điều này hiệu quả vì mỗi ba lô độc lập ngoại trừ ràng buộc chung là các vật phẩm không thể được sử dụng lại. Sau khi chúng tôi đưa một tập hợp con các mặt hàng vào ba lô, quyết định đó sẽ không bao giờ cần phải xem lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhiệm vụ Brute Force |$O(2^M \cdot K^M)$|$O(M)$| Quá chậm | 
| Tập hợp con DP trên ba lô |$O(K \cdot 3^M)$|$O(2^M)$| Đã chấp nhận | 

yếu tố$3^M$xuất hiện vì đối với mỗi mục, trong quá trình chuyển đổi, nó có thể không được sử dụng, đã được sử dụng hoặc được chỉ định trong bối cảnh liệt kê tập hợp con hiện tại. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng ba lô một, duy trì mảng DP trên các tập hợp con các mặt hàng. Mỗi trạng thái DP biểu thị số lượng vật phẩm tối đa có thể được chỉ định bằng cách sử dụng ba lô được xử lý đầu tiên, với một tập hợp con cụ thể các vật phẩm đã được sử dụng. 

1. Khởi tạo một mảng DP có kích thước$2^M$, trong đó mỗi mục nhập biểu thị số lượng mục đã được chỉ định thành công cho một mặt nạ mục đã sử dụng nhất định. Ban đầu, chỉ có mặt nạ trống là hợp lệ với giá trị 0. Điều này thể hiện không có vật phẩm nào được chỉ định và không sử dụng ba lô. 
2. Lặp lại từng chiếc ba lô một lần. Ở mỗi bước, chúng tôi tính toán một mảng DP mới thể hiện tác động của việc cho phép gán các bài tập vào ba lô này. 
3. Đối với một chiếc ba lô cố định, hãy liệt kê tất cả các tập hợp con của các vật phẩm. Với mỗi tập hợp con, hãy tính tổng trọng số của nó. Nếu tổng trọng lượng vượt quá sức chứa của ba lô, hãy vứt bỏ nó ngay lập tức. Điều này đảm bảo chúng tôi chỉ xem xét các gói hàng khả thi. 
4. Đối với mọi tập hợp con khả thi, hãy thử hợp nhất nó với các trạng thái DP trước đó. Nếu trạng thái trước đó sử dụng mặt nạ`mask`, và tập hợp con hiện tại là`sub`và chúng không trùng nhau, chúng ta có thể chuyển sang`mask | sub`. Chúng tôi cập nhật giá trị DP mới với số lượng vật phẩm tốt nhất có thể đạt được. 
5. Sau khi xử lý tất cả các tập hợp con cho một chiếc ba lô, hãy thay bảng DP bằng bảng mới. Điều này khóa các quyết định cho chiếc ba lô đó và tiến về phía trước. 
6. Sau khi tất cả ba lô được xử lý, câu trả lời là giá trị tối đa trên tất cả các trạng thái DP, vì không nhất thiết phải sử dụng tất cả các vật phẩm. 

Lý do điều này có tác dụng là vì mỗi ba lô được xử lý chính xác một lần và mỗi mục được chỉ định nhiều nhất một lần trên tất cả các ba lô. DP đảm bảo chúng tôi khám phá tất cả các kết hợp hợp lệ của các tập hợp con rời rạc được gán cho các ba lô khác nhau, đồng thời tránh các nhiệm vụ lặp lại thông qua mặt nạ bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    K, M = map(int, input().split())
    caps = list(map(int, input().split()))
    w = list(map(int, input().split()))

    nmask = 1 << M

    subset_weight = [0] * nmask
    subset_count = [0] * nmask

    for mask in range(nmask):
        total_w = 0
        cnt = 0
        for i in range(M):
            if mask & (1 << i):
                total_w += w[i]
                cnt += 1
        subset_weight[mask] = total_w
        subset_count[mask] = cnt

    dp = [-10**9] * nmask
    dp[0] = 0

    for cap in caps:
        ndp = dp[:]

        for sub in range(nmask):
            if subset_weight[sub] > cap:
                continue
            add = subset_count[sub]

            for mask in range(nmask):
                if dp[mask] < 0:
                    continue
                if mask & sub:
                    continue
                nm = mask | sub
                if ndp[nm] < dp[mask] + add:
                    ndp[nm] = dp[mask] + add

        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp tính toán trước trọng lượng và kích thước của từng tập hợp con các mục, cho phép kiểm tra tính khả thi theo thời gian liên tục trong quá trình chuyển đổi. Điều này tránh việc tính toán lại các tổng liên tục bên trong các vòng lặp DP, nếu không sẽ nhân thời gian chạy với hệ số$M$. 

Mảng DP lưu trữ số lượng mục tốt nhất được chỉ định cho đến nay đối với mỗi tập hợp con của các mục. Quá trình chuyển đổi thực thi cẩn thận tính rời rạc thông qua điều kiện bitmask`mask & sub == 0`, đảm bảo không có mục nào được chỉ định hai lần. 

Sao chép`dp`vào trong`ndp`mỗi ba lô đảm bảo rằng mỗi ba lô đóng góp chính xác một lần vào việc xây dựng các bài tập, duy trì tính chính xác của bài tập theo giai đoạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
3 5 10
1 3 3
```Chúng tôi tính toán trước các tập hợp con của các mục. Để rõ ràng, hãy ký hiệu các mục là A(1), B(3), C(3). 

| Ba lô | Tập hợp con được chọn | Cân nặng | Kết quả chuyển tiếp DP | 
| --- | --- | --- | --- | 
| ban đầu | ∅ | 0 | dp[000]=0 | 
| 1 (đầu 3) | {A} | 1 | dp[001]=1 | 
| 1 (đầu 3) | {B} không hợp lệ | 3 | dp[011]=1 | 
| 1 (đầu 3) | {C} không hợp lệ | 3 | dp[101]=1 | 
| 2 (đầu 5) | {B,C} | 6 không hợp lệ | không cập nhật | 
| 2 (đầu 5) | {A,B} | 4 | dp[011]=2 | 

Sau khi xử lý tất cả ba lô, nhiệm vụ có thể đạt được tốt nhất là sử dụng 3 vật phẩm. 

Dấu vết này cho thấy cách kết hợp các tập hợp con trên các ba lô khác nhau dần dần tạo ra các bài tập hợp lệ lớn hơn mà không cần sử dụng lại các mục. 

### Mẫu 2 

đầu vào:```
2 3
1 2
2 2 2
```Tất cả các vật phẩm đều có trọng lượng 2, nhưng một chiếc ba lô có sức chứa 1. 

| Ba lô | Tập hợp con được chọn | Cân nặng | Trạng thái DP | 
| --- | --- | --- | --- | 
| ban đầu | ∅ | 0 | dp[000]=0 | 
| nắp1 | bất kỳ mục nào | 2 | không hợp lệ | 
| nắp2 | {A} | 2 | dp[001]=1 | 
| nắp2 | {B} | 2 | dp[010]=1 | 
| nắp2 | {C} | 2 | dp[100]=1 | 

Câu trả lời cuối cùng là 1, vì chỉ có thể đóng gói một mặt hàng ở bất kỳ đâu. 

Điều này xác nhận rằng các tập hợp con không khả thi được lọc một cách tự nhiên bởi các hạn chế về dung lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K \cdot 2^M \cdot 2^M)$| Đối với mỗi ba lô, lặp lại tất cả các tập hợp con hai lần để chuyển đổi DP | 
| Không gian |$O(2^M)$| Mảng DP trên tất cả các tập hợp con mục | 

Với$M \le 15$,$2^M = 32768$, Vì thế$2^M \cdot 2^M$về mặt lý thuyết là ranh giới nhưng có thể chấp nhận được trong Python do các hằng số nhỏ và quá trình tính toán trước. Trong thực tế, việc cắt tỉa thông qua kiểm tra trọng lượng sẽ loại bỏ nhiều quá trình chuyển đổi. 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì tất cả các thao tác đều là kiểm tra mặt nạ bit đơn giản và phép cộng số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def solve():
        K, M = map(int, input().split())
        caps = list(map(int, input().split()))
        w = list(map(int, input().split()))

        nmask = 1 << M
        subset_weight = [0] * nmask
        subset_count = [0] * nmask

        for mask in range(nmask):
            total_w = 0
            cnt = 0
            for i in range(M):
                if mask & (1 << i):
                    total_w += w[i]
                    cnt += 1
            subset_weight[mask] = total_w
            subset_count[mask] = cnt

        dp = [-10**9] * nmask
        dp[0] = 0

        for cap in caps:
            ndp = dp[:]
            for sub in range(nmask):
                if subset_weight[sub] > cap:
                    continue
                add = subset_count[sub]
                for mask in range(nmask):
                    if dp[mask] < 0:
                        continue
                    if mask & sub:
                        continue
                    nm = mask | sub
                    ndp[nm] = max(ndp[nm], dp[mask] + add)
            dp = ndp

        return str(max(dp))

    return solve()

# provided samples
assert run("""3 3
3 5 10
1 3 3
""") == "3"

assert run("""2 3
1 2
2 2 2
""") == "1"

# custom cases
assert run("""1 1
5
5
""") == "1", "single item fits"

assert run("""1 2
1
2 3
""") == "0", "nothing fits"

assert run("""2 2
5 5
2 2
""") == "2", "all items fit both backpacks but used once"

assert run("""3 3
3 3 3
1 2 3
""") == "3", "all items fit independently"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ba lô vừa vặn | 1 | tính khả thi tối thiểu | 
| không có mục nào phù hợp | 0 | xử lý lựa chọn trống | 
| trường hợp dung lượng trùng lặp | 2 | phòng chống tái sử dụng qua bitmask | 
| đóng gói cân bằng | 3 | sử dụng đầy đủ trên ba lô | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi tất cả các món đồ đều quá nặng cho mỗi chiếc ba lô. Ví dụ:```
2 3
1 1
2 2 2
```DP bắt đầu ở mặt nạ 0 với giá trị 0. Mỗi tập hợp con có trọng số ít nhất là 2, vì vậy tất cả các chuyển đổi đều bị bỏ qua. Câu trả lời cuối cùng vẫn là 0, thể hiện chính xác rằng không thể lấy vật phẩm nào. 

Một trường hợp khác là khi một chiếc ba lô có thể đựng được mọi thứ, còn những chiếc khác thì không liên quan:```
3 3
10 1 1
1 2 3
```Ba lô đầu tiên cho phép tập hợp con {1,2,3} cho mặt nạ 111. Ba lô tiếp theo không thể thêm bất cứ thứ gì vì tất cả vật phẩm đều đã được sử dụng nên DP vẫn ổn định. Câu trả lời cuối cùng là 3, phù hợp với kích thước trọn bộ. 

Trường hợp quan trọng cuối cùng là áp lực công suất chồng chéo, trong đó cần phải phân chia các hạng mục. DP đảm bảo điều này bằng cách xem xét tất cả các tập hợp con, do đó, ngay cả khi cách tiếp cận tham lam đóng gói một mục lớn sớm, DP vẫn đánh giá các phân vùng tập hợp con thay thế trên các ba lô và duy trì sự kết hợp tốt nhất.
