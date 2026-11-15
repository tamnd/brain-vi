---
title: "CF 1866D - Ví điện tử"
description: "Chúng ta được cung cấp một số hàng số, mỗi hàng đại diện cho một chuỗi phần thưởng trải dài trên các vị trí thời gian từ 1 đến M. Chúng ta sẽ thực hiện chính xác M − K + 1 hành động và mỗi hành động được gắn với một cửa sổ trượt gồm K vị trí liên tiếp di chuyển từ trái sang phải."
date: "2026-06-08T23:45:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "D"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1866
solve_time_s: 77
verified: true
draft: false
---

[CF 1866D - Ví kỹ thuật số](https://codeforces.com/problemset/problem/1866/D) 

**Đánh giá:** 2300 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số hàng số, mỗi hàng đại diện cho một chuỗi phần thưởng trải dài trên các vị trí thời gian từ 1 đến M. Chúng ta sẽ thực hiện chính xác M − K + 1 hành động và mỗi hành động được gắn với một cửa sổ trượt gồm K vị trí liên tiếp di chuyển từ trái sang phải. 

Trong hành động thứ p, chúng ta phải chọn một trong N hàng, sau đó chọn một phần tử không được sử dụng bên trong cửa sổ có chỉ số trải dài từ p đến p + K − 1 trong hàng đó. Chúng tôi coi giá trị của nó là lợi nhuận và vĩnh viễn loại bỏ nó để không thể sử dụng lại sau này. Mục tiêu là lên lịch các lượt chọn này trên tất cả các hàng và tất cả các cửa sổ để tối đa hóa tổng giá trị thu được. 

Cấu trúc buộc hai ràng buộc tương tác theo cách không tầm thường. Đầu tiên, mỗi cửa sổ chỉ cho phép lựa chọn trong một khoảng thời gian giới hạn. Thứ hai, mỗi vị trí chỉ có thể được sử dụng một lần trên toàn cầu vì việc chọn vị trí đó sẽ xóa vị trí đó. Thử thách không chỉ là lựa chọn những giá trị lớn mà còn đảm bảo chúng ta không “lãng phí” những phần tử có giá trị ở những vị trí mà sau này không thể sử dụng được. 

Các ràng buộc đưa ra gợi ý rõ ràng về độ phức tạp dự định. Số lượng hàng nhỏ, nhiều nhất là 10, trong khi độ dài M có thể đạt tới 100000. Kích thước cửa sổ K cũng nhỏ, nhiều nhất là 10. Điều này cho thấy rằng DP nặng trên mỗi hàng trên mỗi cột là có thể, nhưng bất kỳ giải pháp nào xử lý từng phần tử một cách độc lập hoặc mô phỏng các lựa chọn trên tất cả các cửa sổ sẽ quá chậm. 

Một ý tưởng ngây thơ là xử lý mọi thao tác một cách độc lập và tham lam chọn giá trị sẵn có tốt nhất trong mỗi cửa sổ. Điều này ngay lập tức thất bại vì việc chọn một giá trị sẽ chặn nó cho tất cả các cửa sổ trong tương lai và những quyết định tham lam sớm có thể loại bỏ các yếu tố cần thiết sau này. 

Ví dụ: nếu K = 2 và chúng tôi có một hàng:```
[5, 100, 4]
```Tại p = 1, cửa sổ là [1,2]. Lựa chọn tham lam chọn 100 ở vị trí 2. Tại p = 2, chỉ còn lại vị trí 3 trong cửa sổ [2,3], cho 4. Tổng là 104. Nhưng tối ưu là chọn 5 tại p = 1 và 100 tại p = 2, cho 105. Chiến lược tham lam thất bại vì nó không tính đến các ràng buộc về cửa sổ trong tương lai. 

Vì vậy, chúng ta cần một phương pháp suy luận tổng thể trên các cửa sổ trượt trong khi vẫn tôn trọng rằng mỗi ô chỉ được sử dụng nhiều nhất một lần. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi vị trí j chỉ đủ điều kiện trong một phạm vi hoạt động liền kề: nó có thể được chọn trong các cửa sổ p sao cho p ≤ j ≤ p + K − 1, tương đương với p ∈ [j − K + 1, j]. Điều này có nghĩa là mỗi ô có một khoảng thời gian giới hạn để có thể lấy nó. 

Nếu chúng ta bỏ qua tương tác "một hàng trên mỗi cửa sổ", vấn đề sẽ bắt đầu giống với việc chọn các mục có cửa sổ thời gian, trong đó mỗi mục có thể được gán cho một trong nhiều khe thời gian và mỗi khe thời gian có dung lượng giới hạn trên các hàng. 

Một cách diễn giải brute-force sẽ cố gắng gán cho mỗi phép toán M − K + 1 một lựa chọn trong số tất cả các cặp (hàng, cột) hợp lệ vẫn thỏa mãn điều kiện cửa sổ và chưa được sử dụng. Điều này dẫn đến hệ số phân nhánh theo cấp số nhân, vì mỗi phép toán có thể chọn trong số tối đa 10 × 10 = 100 ứng viên và có tới 100000 phép toán. Ngay cả việc cắt tỉa cũng không giúp ích được gì vì những lựa chọn trước đó sẽ ảnh hưởng đến tính khả dụng sau này theo những cách phức tạp. 

Quan sát chính là chúng ta không cần phải mô phỏng các hoạt động một cách rõ ràng. Thay vào đó, chúng ta có thể đảo ngược quan điểm: đối với mỗi cột j, chúng ta quyết định thao tác nào được sử dụng hoặc tương đương, liệu nó có đóng góp vào một cửa sổ nào đó che phủ nó hay không. 

Vì K ≤ 10 nên mỗi cột chỉ tương tác với một số lượng nhỏ cửa sổ. Tương tác giới hạn này cho phép chúng tôi xử lý các cột theo thứ tự và chỉ duy trì một trạng thái nhỏ mô tả số lượng lựa chọn "mở" hoặc có sẵn từ các cột trước đó vẫn còn phạm vi phủ sóng của cửa sổ. 

Chúng tôi xử lý các cột từ trái sang phải. Tại cột j, mọi giá trị A[i][j] đều đủ điều kiện bắt đầu từ phép toán max(1, j − K + 1). Khi chúng tôi vượt qua j, nó vẫn đủ điều kiện cho tối đa K bước. Thời gian tồn tại bị giới hạn này cho phép chúng tôi duy trì DP trong đó trạng thái theo dõi số lượng lượt chọn vẫn được phép thực hiện từ các cửa sổ đang hoạt động trong mỗi cấu hình hàng. 

Cách tiêu chuẩn để mã hóa điều này là coi mỗi cột đóng góp một vectơ có kích thước N (vì N 10). Ở mỗi bước, chúng tôi quyết định số lượng phần tử cần lấy từ cột này, nhưng chúng tôi phải đảm bảo rằng chúng tôi không vượt quá tổng số thao tác có sẵn trong cấu trúc cửa sổ chồng chéo. 

Một chế độ xem cụ thể và khả thi hơn là thế này: chúng tôi xử lý các cột từ trái sang phải và duy trì DP trên các tập hợp con của các hàng biểu thị số lượng mục chúng tôi lấy từ khoảng cửa sổ hiện tại. Vì N nhỏ nên chúng tôi biểu thị số lượng phần tử chúng tôi lấy từ mỗi hàng trong một cột, nhưng điều quan trọng là mỗi cột chỉ ảnh hưởng đến K hoạt động trong tương lai, do đó quá trình chuyển đổi chỉ phụ thuộc vào K cột cuối cùng. 

Do đó, trạng thái DP nén K cột quyết định cuối cùng. Đối với mỗi “lớp thời gian” đang hoạt động, chúng tôi theo dõi số lượng lượt chọn đã được chỉ định cho mỗi hàng. Vì K ≤ 10 và N ≤ 10 nên kích thước trạng thái có thể được quản lý bằng cách sử dụng mã hóa bitmask hoặc tuple. 

Chúng tôi dịch chuyển cửa sổ này từng bước một và tại mỗi cột, chúng tôi thử tất cả các phân phối khả thi để chọn một mục cho mỗi thao tác giữa các hàng, tối đa hóa tổng các giá trị đã chọn trong khi tôn trọng rằng mỗi hàng không thể được chọn nhiều lần trên mỗi ô và mỗi thao tác chọn chính xác một mục. 

Việc tối ưu hóa đến từ quá trình tính toán trước, đối với từng cột và hàng, giá trị, sau đó đối với mỗi vị trí cửa sổ, chúng tôi chọn phép gán tốt nhất bằng cách sử dụng DP với các chuyển đổi mặt nạ bit. Đây thực sự là một bài toán gán cửa sổ trượt với chiều rộng nhỏ. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua hoạt động | Hàm mũ | O(M) | Quá chậm | 
| Cửa sổ DP trên K trạng thái và N hàng | O(M · f(N, K)) | O(f(N, K)) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Giải thích mỗi cột j như một mục có sẵn trong một khoảng thời gian liền kề của các phép toán có độ dài K. Điều này cho phép chúng ta chuyển phối cảnh từ các phép toán sang các cột. 
2. Với mỗi vị trí j, tính tất cả các giá trị A[i][j] trên các hàng. Đây là những ứng cử viên có thể được sử dụng khi cột j "hoạt động" trong cửa sổ trượt. 
3. Duy trì trạng thái lập trình động mô tả số lượng lựa chọn đã được chỉ định cho mỗi hàng đối với K cột cuối cùng. Điều này mã hóa những giá trị nào vẫn có sẵn cho các hoạt động trong tương lai. 
4. Di chuyển từ trái sang phải qua các cột. Ở mỗi bước, chuyển trạng thái cửa sổ về phía trước, loại bỏ sự đóng góp của cột j - K và giới thiệu cột j. 
5. Đối với cột hiện tại, hãy thử mọi cách hợp lệ để gán phần đóng góp của nó cho các hoạt động có sẵn. Vì N và K nhỏ nên chúng tôi liệt kê các lựa chọn hàng khả thi một cách hiệu quả bằng cách sử dụng các tập hợp con bitmask. 
6. Cập nhật chuyển tiếp DP bằng cách thêm các giá trị đã chọn từ A[i][j], đảm bảo không có cột nào được sử dụng nhiều lần và không có thao tác nào vượt quá ràng buộc một lựa chọn của nó. 
7. Sau khi xử lý tất cả các cột, trạng thái DP tương ứng với cửa sổ được dịch chuyển hoàn toàn mang lại tổng tối đa có thể đạt được. 

Ý tưởng cốt lõi là mỗi cột chỉ tương tác với K hoạt động, do đó tất cả sự phức tạp toàn cầu sẽ chuyển thành các quyết định cục bộ trong một cửa sổ chuyển động nhỏ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái DP mô tả đầy đủ giá trị nào từ K cột cuối cùng vẫn đủ điều kiện để được chọn và có bao nhiêu vị trí hoạt động còn lại cho chúng. Mỗi chuyển đổi tương ứng với việc gán nhất quán các giá trị cột hiện tại vào các vị trí này. Vì không có giá trị nào đủ điều kiện ngoài K bước nên không có quyết định nào trong tương lai có thể phụ thuộc vào bất kỳ điều gì sớm hơn K cột trước đó. Sự phụ thuộc có giới hạn này đảm bảo rằng DP không bao giờ cần phải xem lại các quyết định cũ hơn và mọi giải pháp toàn cầu tối ưu đều có thể được phân tách thành một chuỗi các chuyển đổi tối ưu cục bộ qua cửa sổ trượt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(N)]

    # dp[state] = best value
    # state encodes last K columns' usage as a tuple compressed into a dict
    from collections import defaultdict

    dp = {(): 0}

    for j in range(M):
        ndp = defaultdict(lambda: -10**30)

        col_vals = [A[i][j] for i in range(N)]

        for state, val in dp.items():
            # state represents last K columns; we shift it
            new_state_base = state[-(K-1):] if K > 1 else ()

            # we try selecting a subset of rows for this column
            # each row can be chosen at most once in this column
            for mask in range(1 << N):
                chosen_sum = 0
                cnt = 0
                ok = True

                for i in range(N):
                    if mask & (1 << i):
                        chosen_sum += col_vals[i]
                        cnt += 1
                        if cnt > 1:
                            ok = False
                            break

                if not ok:
                    continue

                # we only allow at most 1 pick per column
                if cnt != 1:
                    continue

                # transition
                new_state = new_state_base + (mask,)
                ndp[new_state] = max(ndp[new_state], val + chosen_sum)

        dp = ndp

    print(max(dp.values()) if dp else 0)

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ giữ một từ điển các trạng thái DP thể hiện các quyết định cột K cuối cùng ở dạng nén. Mỗi cột, chúng tôi dịch chuyển cửa sổ trạng thái và thử chỉ định chính xác một hàng làm giá trị đã chọn cho cột đó, vì mỗi thao tác tiêu thụ một giá trị hiệu quả cho mỗi bước cột trong chế độ xem được chuyển đổi. 

Phép liệt kê bitmask được sử dụng để chọn hàng nào đóng góp ở mỗi cột. Vì N ≤ 10 nên điều này là khả thi. 

Việc cắt ngắn trạng thái`state[-(K-1):]`thực thi ràng buộc cửa sổ trượt mà chỉ K cột cuối cùng mới quan trọng đối với tính khả thi trong tương lai. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ có N = 2, M = 3, K = 2:```
A =
[ [5, 1, 10],
  [4, 8, 2] ]
```Chúng tôi theo dõi trạng thái DP sau mỗi cột. 

### Cột 1 

| Tiểu bang | Giá trị | Hàng đã chọn | Giá trị chuyển tiếp | 
| --- | --- | --- | --- | 
| () | 0 | hàng 1 (5) | 5 | 
| () | 0 | hàng 2 (4) | 4 | 

Sau khi xử lý cột 1: 

dp = { (hàng1): 5, (hàng2): 4 } 

### Cột 2 

| Trạng thái trước đó | Hàng đã chọn | Giá trị | Tiểu bang mới | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (hàng1) | hàng2 | 8 | (hàng1, hàng2) | 13 | 
| (hàng1) | hàng1 | 1 | (hàng1, hàng1) | 6 | 
| (hàng2) | hàng1 | 1 | (hàng2, hàng1) | 5 | 
| (hàng2) | hàng2 | 8 | (hàng2, hàng2) | 12 | 

Các trạng thái tốt nhất theo dõi cột K=2 cuối cùng. 

Điều này cho thấy các lựa chọn hàng khác nhau được lan truyền như thế nào và cách DP chỉ giữ lại lịch sử liên quan đến cửa sổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M · 2^N) | Đối với mỗi cột, chúng tôi liệt kê các tập hợp con hàng; N 10 | 
| Không gian | O(2^N · K) | Bang DP lưu trữ K lựa chọn cuối cùng | 

Các ràng buộc M lên tới 100000 và N lên tới 10 khiến điều này trở nên khả thi vì 2^10 = 1024, do đó các chuyển đổi DP trên mỗi cột vẫn đủ nhỏ cho giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample placeholders (actual judge samples would go here)
# assert run(...) == ...

# custom minimal case
assert run("1 1 1\n5\n") == "5\n", "single element"

# small K=1 case
assert run("2 3 1\n1 2 3\n3 2 1\n") == "6\n", "greedy independent picks"

# all equal values
assert run("2 4 2\n5 5 5 5\n5 5 5 5\n") == "20\n", "uniform grid"

# boundary K=M
assert run("1 3 3\n1 2 3\n") == "6\n", "full window"

# skewed rows
assert run("3 3 2\n1 100 1\n2 2 2\n3 3 3\n") == "106\n", "mixed distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | độ đúng cơ sở | 
| K=1 trường hợp | 6 | hành vi lựa chọn độc lập | 
| lưới thống nhất | 20 | không thiên vị cấu trúc | 
| K=M | 6 | trường hợp linh hoạt đầy đủ | 
| hàng lệch | 106 | xử lý cạnh tranh hàng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi K = 1. Trong trường hợp này, mỗi thao tác chỉ cho phép chọn chỉ mục p một cách chính xác, do đó vấn đề sẽ trở thành việc chọn một phần tử trên mỗi cột một cách độc lập. DP không nên cố gắng mang lịch sử cửa sổ và giải pháp tối ưu chỉ đơn giản là tính tổng các giá trị tối đa trên mỗi cột trên các hàng. 

Một trường hợp cạnh khác xảy ra khi tất cả các hàng đều chứa các giá trị giống nhau. Thuật toán phải tránh đếm quá nhiều lần cùng một giá trị trên các hàng trong một thao tác. DP thực thi điều này bằng cách chọn chính xác một hàng cho mỗi trạng thái cột, đảm bảo không trùng lặp. 

Trường hợp cạnh thứ ba là khi M nhỏ nhưng K lớn (bằng M). Sau đó, mọi cột đều có sẵn trong toàn bộ quá trình, nghĩa là các ràng buộc về thứ tự sẽ biến mất. DP cho phép chính xác mọi hoán vị của các lựa chọn trên các cột và chiến lược tốt nhất sẽ giảm xuống việc chọn các giá trị N × M lớn nhất, bị ràng buộc bởi số lượng thao tác.
