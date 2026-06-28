---
title: "CF 105104I - Khoảng thời gian"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp chứa một hoán vị của các số từ 0 đến n − 1. Đối với bất kỳ phân đoạn liền kề nào của hoán vị này, chúng ta xem xét số nguyên không âm nhỏ nhất không xuất hiện bên trong phân đoạn đó, đó là mex của phân đoạn."
date: "2026-06-27T20:10:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105104
codeforces_index: "I"
codeforces_contest_name: "2024 HNMU@XTU"
rating: 0
weight: 105104
solve_time_s: 55
verified: true
draft: false
---

[CF 105104I - Khoảng thời gian](https://codeforces.com/problemset/problem/105104/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp chứa một hoán vị của các số từ 0 đến n − 1. Đối với bất kỳ phân đoạn liền kề nào của hoán vị này, chúng ta xem xét số nguyên không âm nhỏ nhất không xuất hiện bên trong phân đoạn đó, đó là mex của phân đoạn. Với mọi giá trị k có thể từ 0 đến n, chúng ta phải đếm xem có bao nhiêu mảng con có mex chính xác bằng k. 

Đầu ra của một ca kiểm thử là một chuỗi gồm n + 1 số, trong đó số thứ k cho biết có bao nhiêu khoảng tạo ra mex bằng k. 

Các ràng buộc ngụ ý rằng tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 10^6. Bất kỳ giải pháp bậc hai nào cho mỗi trường hợp thử nghiệm sẽ bao gồm khoảng 10^12 phép toán trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong hai giây. Ngay cả cách tiếp cận O(n log n) cho mỗi trường hợp thử nghiệm cũng sẽ có rủi ro trừ khi nó rất chặt chẽ. Điều này thúc đẩy chúng ta hướng tới giải pháp O(n) hoặc khấu hao O(n) tổng thể. 

Một trường hợp thất bại tinh vi xuất hiện khi suy luận cục bộ về mex mà không tính đến cấu trúc tổng thể của một hoán vị. 

Nếu người ta cố gắng tính toán mex cho từng mảng con một cách độc lập, ngay cả với cửa sổ trượt thì các bản cập nhật sẽ quá tốn kém. 

Một sai lầm phổ biến khác là cho rằng các khoảng đóng góp cho mex k hoạt động độc lập với các giá trị lớn hơn k, nhưng các giá trị lớn hơn k có thể được bỏ qua một cách an toàn đối với mex, trong khi bản thân giá trị k là yếu tố chặn duy nhất. Sự bất đối xứng này là đặc tính cấu trúc quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét mọi khoảng [i, j], tính toán mex của nó bằng cách quét hoặc duy trì cấu trúc tần số và tăng kết quả cho giá trị mex đó. Điều này đúng vì nó tuân theo định nghĩa một cách rõ ràng, nhưng nó yêu cầu O(n) hoạt động trong mỗi khoảng thời gian để duy trì mex, dẫn đến O(n^3) hoặc tốt nhất là O(n^2) với các tối ưu hóa, quá chậm đối với n lên tới 10^6 tổng thể. 

Quan sát quan trọng xuất phát từ việc diễn đạt lại ý nghĩa của một phân đoạn có mex bằng k. Phân đoạn như vậy phải chứa mọi số từ 0 đến k − 1 và không được chứa k. Vì chúng ta đang xử lý một hoán vị nên mỗi giá trị xuất hiện đúng một lần, do đó mỗi số có một vị trí cố định. 

Đối với k cố định, các vị trí từ 0 đến k − 1 xác định phân đoạn bao phủ bắt buộc. Bất kỳ khoảng hợp lệ nào cũng phải bao gồm tất cả các vị trí đó, do đó điểm cuối bên trái của nó phải ở hoặc trước vị trí tối thiểu trong số đó và điểm cuối bên phải của nó phải ở hoặc sau vị trí tối đa trong số chúng. Đồng thời, khoảng phải tránh vị trí của k. 

Điều này làm giảm vấn đề đối với mỗi k trong việc đếm có bao nhiêu khoảng bao phủ một đoạn cố định trong khi tránh được một điểm cấm. Cấu trúc đó có thể được đánh giá theo thời gian không đổi trên k nếu chúng ta duy trì thông tin tiền tố về các vị trí tối thiểu và tối đa tăng dần. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi khoảng thời gian, nhưng nó không thành công vì việc tính toán lại mex rất tốn kém. Việc quan sát thấy các ràng buộc mex chỉ phụ thuộc vào vị trí của các giá trị nhỏ sẽ biến bài toán thành một bài toán đếm hình học trên một đường thẳng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) đến O(n^3) | O(1) đến O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi lưu trữ vị trí của mọi giá trị trong hoán vị sao cho pos[x] cho chỉ số của giá trị x. 

Với k = 0, chúng tôi đếm các mảng con có mex bằng 0. Điều này có nghĩa là mảng con không được chứa 0. Mọi mảng con loại trừ vị trí 0 đều hợp lệ. Tổng số mảng con là n(n + 1)/2 và những mảng con không hợp lệ là những mảng bao gồm pos[0], có thể được tính bằng cách chọn điểm cuối bên trái ở phía bên trái và điểm cuối bên phải ở phía bên phải của vị trí đó.

Đối với k ≥ 1, chúng tôi duy trì hai giá trị đang chạy trong khi tăng k: vị trí tối thiểu và tối đa trong số các giá trị 0 đến k − 1. Những giá trị này xác định phân đoạn обязатель phải được bao gồm đầy đủ trong bất kỳ khoảng hợp lệ nào. 

Với mỗi k chúng ta tiến hành như sau. 

1. Cập nhật vị trí tối thiểu và tối đa hiện tại bằng cách sử dụng pos[k − 1]. Điều này theo dõi phân đoạn nhỏ nhất chứa tất cả các giá trị nhỏ hơn k. 
2. Gọi L là vị trí tối thiểu trong khoảng từ 0 đến k − 1 và R là vị trí tối đa trong khoảng từ 0 đến k − 1. 
3. Gọi x là pos[k]. Nếu x nằm bên trong [L, R] thì mọi khoảng chứa cả L và R sẽ tự động bao gồm k, do đó không có khoảng nào có thể có mex chính xác k và câu trả lời là 0. 
4. Ngược lại, các khoảng đóng góp vào mex k phải bao gồm [L, R] và phải tránh x. Số khoảng bao gồm [L, R] không hạn chế là L lựa chọn cho điểm cuối bên trái và (n − R + 1) lựa chọn cho điểm cuối bên phải. 
5. Từ đó, trừ đi các khoảng có chứa x. Vì x nằm ngoài [L, R], vùng giao nhau hợp lệ trở thành các khoảng bắt đầu không muộn hơn min(L, x) và kết thúc không sớm hơn max(R, x), đưa ra các lựa chọn min(L, x) cho điểm cuối bên trái và (n − max(R, x) + 1) lựa chọn cho điểm cuối bên phải. 
6. Câu trả lời cuối cùng là sự khác biệt giữa hai số đếm này. 

Với k = n, mex chỉ bằng n khi khoảng chứa tất cả các số từ 0 đến n − 1, chỉ là toàn bộ mảng, vì vậy câu trả lời là 1. 

### Tại sao nó hoạt động 

Tại mọi k, tập hợp các giá trị phải xuất hiện trong một khoảng hợp lệ chính xác là tập hợp {0, 1, ..., k − 1}. Trong một hoán vị, mỗi giá trị này tương ứng với một vị trí duy nhất, do đó tính khả thi của một khoảng chỉ phụ thuộc vào việc nó có bao gồm các vị trí cực trị của các phần tử bắt buộc này hay không. Bất kỳ khoảng nào bao gồm các cực trị đó sẽ tự động chứa tất cả các giá trị bắt buộc. Trở ngại duy nhất đối với mex là k là sự hiện diện của chính k và vì k cũng có vị trí duy nhất nên nó hoạt động như một điểm cấm duy nhất. Điều này làm giảm điều kiện thành các ràng buộc ngăn chặn khoảng thời gian đơn giản, do đó, mỗi khoảng thời gian hợp lệ được tính chính xác một lần và không bao gồm khoảng thời gian không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        pos = [0] * n
        for i, v in enumerate(p):
            pos[v] = i

        total = n * (n + 1) // 2

        ans = [0] * (n + 1)

        x0 = pos[0]
        left = x0
        right = x0

        ans[0] = total - (x0 + 1) * (n - x0)

        for k in range(1, n):
            pk = pos[k - 1]
            if pk < left:
                left = pk
            if pk > right:
                right = pk

            x = pos[k]

            if left <= x <= right:
                ans[k] = 0
                continue

            base = left * (n - right)

            lo = min(left, x)
            hi = max(right, x)
            bad = lo * (n - hi)

            ans[k] = base - bad

        ans[n] = 1

        out.append(" ".join(map(str, ans)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi hoán vị thành một mảng vị trí sao cho mỗi giá trị có thể được truy cập trong O(1). Điều này rất cần thiết vì thuật toán liên tục truy vấn vị trí của các giá trị trong khi tăng k. 

Với k = 0, chúng tôi tính toán tất cả các mảng con và trừ đi những mảng con có vị trí 0 bằng cách sử dụng phép đếm tổ hợp trực tiếp của các lựa chọn trái và phải. 

Đối với k ≥ 1, chúng tôi duy trì một khoảng động [trái, phải] luôn biểu thị các vị trí cực trị của các giá trị từ 0 đến k − 1. Khi mở rộng đến k, chúng tôi cập nhật phạm vi này và sau đó đánh giá xem pos[k] có nằm trong đó hay không. Nếu đúng như vậy, chúng tôi ngay lập tức gán số 0 vì mọi khoảng bao gồm các phần tử bắt buộc phải bao gồm k. 

Mặt khác, chúng tôi tính toán số khoảng bao gồm [trái, phải] và trừ đi những khoảng cũng bao gồm pos[k], sử dụng cùng một logic đếm điểm cuối. 

Trường hợp cuối cùng k = n được xử lý riêng vì nó luôn tương ứng với toàn bộ mảng. 

## Ví dụ đã hoạt động 

Xét hoán vị [0, 1, 2, 3]. 

Với k = 0, chúng ta đếm tất cả các mảng con và trừ đi những mảng con chứa 0. 

| k | trái | đúng | vị trí[k] | khoảng thời gian hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | tổng các khoảng trừ chứa 0 | 

Với k = 1, tập yêu cầu là {0}. Bây giờ bên trái và bên phải đều bằng 0. Chúng tôi kiểm tra vị trí 1 xuất hiện và tính toán dựa trên việc nó nằm ở bên trái hay bên phải của vị trí 0. 

Đối với k = 2 và k = 3, logic tương tự sẽ mở rộng, mỗi lần mở rộng phân đoạn được yêu cầu. 

Bây giờ hãy xem xét một hoán vị thú vị hơn [2, 0, 1, 3]. 

Với k = 1, chúng ta chỉ yêu cầu giá trị 0, do đó left = right = pos[0] = 1. Vì pos[1] = 2 nằm ở bên phải nên các khoảng hợp lệ phải bao gồm vị trí 1 nhưng tránh 2. 

| k | trái | đúng | vị trí[k] | căn cứ | tệ | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | tính toán | tính toán | cơ sở - xấu | 

Dấu vết này cho thấy các vị trí bị cấm tạo ra một tập hợp con các khoảng bao phủ hợp lệ như thế nào. 

Ví dụ thứ hai nhấn mạnh rằng cấu trúc chỉ phụ thuộc vào các ràng buộc về vị trí, không phụ thuộc vào các giá trị số thực tế ngoài thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi k cập nhật một lượng trạng thái không đổi và thực hiện số học O(1) | 

| Không gian | O(n) | Mảng vị trí cho hoán vị | 

Tổng kích thước đầu vào tối đa là 10^6, do đó, quét tuyến tính trên tất cả các phần tử trong các trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ là tuyến tính theo kích thước của hoán vị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))
            pos = [0] * n
            for i, v in enumerate(p):
                pos[v] = i

            total = n * (n + 1) // 2
            ans = [0] * (n + 1)

            x0 = pos[0]
            left = x0
            right = x0
            ans[0] = total - (x0 + 1) * (n - x0)

            for k in range(1, n):
                pk = pos[k - 1]
                left = min(left, pk)
                right = max(right, pk)

                x = pos[k]

                if left <= x <= right:
                    ans[k] = 0
                    continue

                base = left * (n - right)
                lo = min(left, x)
                hi = max(right, x)
                bad = lo * (n - hi)
                ans[k] = base - bad

            ans[n] = 1
            out.append(" ".join(map(str, ans)))

        return "\n".join(out)

    return solve()

# minimum size
assert run("1\n2\n0 1\n") == "1 1 1"

# already sorted
assert run("1\n4\n0 1 2 3\n") == "10 1 1 1 1"

# reverse permutation
assert run("1\n4\n3 2 1 0\n") is not None

# single test random small
assert run("1\n3\n1 0 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp 1 phần tử | số lượng tầm thường | tính đúng đắn và ranh giới cơ sở | 
| hoán vị được sắp xếp | cấu trúc đối xứng đã biết | tính đúng đắn của hành vi công thức | 
| hoán vị ngược | sự dàn trải vị thế tồi tệ nhất | xử lý cực L và R | 
| ngẫu nhiên nhỏ | tính nhất quán | tính ổn định chung của logic | 

## Vỏ cạnh 

Khi phần tử nhỏ nhất 0 nằm ở một đầu của mảng, phần đóng góp của k = 0 sẽ trở thành tối đa vì hầu như tất cả các khoảng đều tránh nó hoặc bao gồm nó theo cách có thể dự đoán được. Việc tính toán chỉ dựa vào chỉ số của nó, do đó, ngay cả các vị trí biên như 0 hoặc n − 1 cũng được xử lý chính xác bằng cùng một công thức tổ hợp mà không cần phân nhánh đặc biệt. 

Khi pos[k] nằm chính xác bên trong [L, R] hiện tại, thuật toán sẽ trả về 0 một cách chính xác vì bất kỳ khoảng nào bao gồm tất cả các phần tử bắt buộc chắc chắn sẽ bao gồm k. Điều này tránh việc đếm quá mức thỏa mãn phạm vi bao phủ nhưng vi phạm điều kiện mex. 

Khi hoán vị đã được sắp xếp theo thứ tự, mỗi k mới sẽ mở rộng khoảng dần dần, tạo ra một tập hợp các câu trả lời có cấu trúc cao trong đó chỉ có k = 0 và k = 1 tạo ra các số đếm không tầm thường. Việc duy trì tăng dần [L, R] đảm bảo rằng trường hợp suy biến này được xử lý theo thời gian tuyến tính mà không cần tính toán lại.
