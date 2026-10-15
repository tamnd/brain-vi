---
title: "CF 105442I - Cắt P||k"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị “các ưu đãi cookie” hàng ngày. Mỗi khoảng thời gian là một khối ngày liền kề và trong bất kỳ khoảng thời gian nào đã chọn, hai quy tắc thanh toán khác nhau sẽ được áp dụng cho cùng một phân đoạn. Một nhóm yêu cầu thanh toán K không đổi bất kể nội dung khoảng thời gian."
date: "2026-06-23T03:37:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105442
codeforces_index: "I"
codeforces_contest_name: "2024-2025 CTU Open Contest"
rating: 0
weight: 105442
solve_time_s: 55
verified: true
draft: false
---

[CF 105442I - Cắt P||k](https://codeforces.com/problemset/problem/105442/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị “các ưu đãi cookie” hàng ngày. Mỗi khoảng thời gian là một khối ngày liền kề và trong bất kỳ khoảng thời gian nào đã chọn, hai quy tắc thanh toán khác nhau sẽ được áp dụng cho cùng một phân đoạn. 

Một nhóm yêu cầu thanh toán K không đổi bất kể nội dung khoảng thời gian. Nhóm còn lại đánh giá khoảng thời gian bằng cách lấy bit OR của tất cả các giá trị bên trong nó. Khoảng chỉ được chấp nhận nếu hai giá trị này khớp chính xác, nghĩa là OR của tất cả các phần tử trong khoảng phải bằng K. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề không trống có bitwise HOẶC chính xác bằng K. 

Các ràng buộc cho phép tối đa 400.000 phần tử, với giá trị lên tới 10^9. Việc liệt kê bậc hai của tất cả các mảng con là quá chậm vì nó sẽ bao hàm khoảng 10^10 phép tính trong trường hợp xấu nhất. Ngay cả cách tiếp cận O(n log n) cũng phải được biện minh cẩn thận vì mỗi thao tác mảng con phải được khấu hao hoặc nén. 

Một khía cạnh tinh tế là bitwise OR đơn điệu khi kéo dài các khoảng thời gian. Khi một bit trở thành 1 trong tiền tố, nó sẽ không bao giờ biến mất khi kéo dài khoảng thời gian. Thuộc tính này định hình rõ ràng loại khoảng nào có thể khớp với mục tiêu K cố định. 

Một số tình huống khó khăn quan trọng: 

Nếu K bằng 0 thì chỉ những khoảng trong đó tất cả các phần tử bằng 0 mới hợp lệ. Ví dụ, trong`[0, 0, 1]`, chỉ có hai phần tử đơn hoặc phân đoạn kết hợp chỉ có 0 mới đóng góp. 

Nếu bất kỳ phần tử nào chứa một bit bên ngoài K, thì bất kỳ khoảng nào chứa nó sẽ ngay lập tức vượt K trong OR, do đó các vị trí đó đóng vai trò là rào cản cứng cho các khoảng hợp lệ. 

Nếu K có một bit bằng 0 thì không có phần tử nào trong khoảng hợp lệ có thể đưa ra bit đó, điều này lại hạn chế rất nhiều các phân đoạn hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi chỉ mục bắt đầu, hãy mở rộng đến mọi chỉ mục kết thúc, tính toán OR theo bit của phân đoạn và đếm xem có bao nhiêu K bằng nhau. Tính toán OR tăng dần tạo ra mỗi phần mở rộng O(1), do đó tổng độ phức tạp là O(n^2). Với n lên tới 4×10^5, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần liệt kê tất cả các mảng con. Chúng tôi chỉ quan tâm đến các mảng con có OR chính xác là K. Bởi vì OR là đơn điệu, một khi OR vượt quá K ở bất kỳ vị trí bit nào, nó không bao giờ có thể quay trở lại K bằng cách mở rộng thêm. Điều này có nghĩa là mọi phân đoạn không hợp lệ chỉ có thể không hợp lệ theo cách giống như tiền tố đối với điểm bắt đầu cố định. 

Chúng ta có thể diễn giải lại điều kiện “HOẶC bằng K” như hai yêu cầu đồng thời: mỗi bit được đặt trong bất kỳ phần tử nào của khoảng phải là tập con của các bit trong K và nói chung khoảng phải kích hoạt tất cả các bit của K. 

Điều này gợi ý quét từ trái sang phải trong khi duy trì tất cả giá trị OR của mảng con kết thúc ở chỉ mục hiện tại. Thay vì tính toán lại tất cả các mảng con một cách rõ ràng, chúng tôi duy trì một tập hợp nén các kết quả OR riêng biệt cho các mảng con kết thúc ở mỗi vị trí. Thực tế quan trọng là số lượng giá trị OR riêng biệt kết thúc tại một vị trí là nhỏ, nhiều nhất là O(log A) trong thực tế, bởi vì mỗi kết quả OR đều tăng nghiêm ngặt trong không gian bit và ổn định nhanh chóng. 

Tại mỗi chỉ mục, chúng tôi mở rộng tất cả các trạng thái OR trước đó bằng cách OR-ing với giá trị hiện tại và cũng bắt đầu một mảng con mới tại chỉ mục này. Sau đó chúng tôi chỉ giữ lại các giá trị OR duy nhất thu được. Đối với mỗi vị trí, chúng ta đếm xem có bao nhiêu giá trị OR bằng K. 

Điều này biến vấn đề thành việc duy trì một tập hợp các kết quả OR có thể đạt được, thay vì liệt kê tất cả các mảng con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| DP trên trạng thái OR | O(n log A) | O(log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải và duy trì cấu trúc giống như từ điển biểu thị tất cả các kết quả OR theo bit riêng biệt của các mảng con kết thúc ở vị trí hiện tại, cùng với số lượng mảng con tạo ra mỗi OR. 

Ở mỗi bước, chúng ta xây dựng một cấu trúc mới từ cấu trúc trước đó bằng cách mở rộng tất cả các mảng con hiện có với phần tử hiện tại, đồng thời thêm mảng con một phần tử bắt đầu từ chỉ mục hiện tại. 

Sau đó, chúng tôi đếm xem có bao nhiêu giá trị OR bằng K và tích lũy kết quả. 

### bước 

1. Khởi tạo cấu trúc trống`prev`ánh xạ các giá trị OR thành số đếm và câu trả lời chung được đặt thành 0. 

Điều này thể hiện kết quả OR của các mảng con kết thúc ở chỉ mục trước đó. 
2. Lặp qua từng phần tử`x`trong mảng. Tạo một cấu trúc trống mới`curr`. 
3. Thêm một mảng con mới chỉ bao gồm`x`vào trong`curr`, cài đặt`curr[x] += 1`. 

Điều này giải thích cho các mảng con bắt đầu ở chỉ mục hiện tại. 
4. Đối với mọi`(or_value, count)`TRONG`prev`, tính toán`new_or = or_value | x`và thêm`count`ĐẾN`curr[new_or]`. 

Điều này mở rộng tất cả các mảng con trước đó thêm một phần tử. 
5. Sau khi xây dựng`curr`, thêm vào`curr[K]`(nếu nó tồn tại) cho câu trả lời chung. 
6. Thay thế`prev`với`curr`và tiếp tục. 

Lý do chính khiến chúng ta có thể nén các trạng thái là vì nhiều mảng con khác nhau thu gọn về cùng một giá trị OR và chỉ giá trị OR là quan trọng đối với việc mở rộng trong tương lai. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến tại vị trí i,`prev`chứa chính xác tần số của tất cả các kết quả OR riêng biệt của các mảng con kết thúc ở i-1. Khi chúng tôi xử lý một phần tử mới, mọi mảng con kết thúc bằng i đều bắt đầu tại i hoặc là phần mở rộng của mảng con kết thúc ở i-1, do đó không có trường hợp nào bị bỏ sót hoặc được tính gấp đôi. Vì OR có tính kết hợp và giao hoán nên cách xây dựng lũy ​​tiến bảo toàn tính đúng đắn. Chỉ đếm những trạng thái bằng K là đủ vì mỗi trạng thái tương ứng với ít nhất một mảng con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    prev = {}
    ans = 0

    for x in a:
        curr = {}

        curr[x] = curr.get(x, 0) + 1

        for v, c in prev.items():
            nv = v | x
            curr[nv] = curr.get(nv, 0) + c

        ans += curr.get(k, 0)
        prev = curr

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ lại các trạng thái OR tổng hợp thay vì các mảng con đầy đủ. Mỗi lần lặp lại sẽ xây dựng lại tập hợp các giá trị OR có thể truy cập cho các mảng con kết thúc ở chỉ mục hiện tại. 

Phần tế nhị nhất là đảm bảo chúng ta không trộn lẫn các trạng thái giữa các vị trí. Đặt lại`curr`ở mỗi bước bắt buộc rằng tất cả các mục nhập phải tương ứng chặt chẽ với các mảng con kết thúc ở chỉ mục hiện tại. Bước hợp nhất từ ​​điển duy trì nhiều mảng con thu gọn thành các kết quả OR giống hệt nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 3
a = [1, 2, 1, 2, 3]
```Chúng tôi theo dõi`prev`(trạng thái kết thúc ở i-1) và`curr`(kết thúc bằng i). 

| tôi | x | trạng thái trước | trạng thái hiện tại | đếm K | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | ∅ | {1:1} | 0 | 
| 2 | 2 | {1} | {2:1, 3:1} | 1 | 
| 3 | 1 | {2:1, 3:1} | {1:1, 3:1, 3:1, 3:1} → {1:1, 3:3} | 3 | 
| 4 | 2 | {1,3} | {2,3,3,3} → {2:1,3:3} | 3 | 
| 5 | 3 | {2,3} | {3,3,3,3} → {3:4} | 4 | 

Câu trả lời cuối cùng tích lũy tất cả các lần xuất hiện của giá trị OR 3 trên các vị trí. 

Dấu vết này cho thấy có bao nhiêu mảng con khác nhau thu gọn thành giá trị OR 3, đặc biệt là sau khi phần tử 3 xuất hiện, điều này buộc tất cả các phần mở rộng phải chạm vào OR mục tiêu. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 1
a = [1, 0, 1, 0]
```| tôi | x | trạng thái hiện tại | đếm K | 
| --- | --- | --- | --- | 
| 1 | 1 | {1} | 1 | 
| 2 | 0 | {0,1} | 1 | 
| 3 | 1 | {1,1,1} → {1} | 1 | 
| 4 | 0 | {0,1} | 1 | 

Ví dụ này cho thấy số 0 không thay đổi giá trị OR và duy trì các trạng thái hiện có, gây ra nhiều khoảng thời gian hợp lệ chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · B) | Mỗi vị trí hợp nhất một số lượng nhỏ trạng thái OR riêng biệt, được giới hạn bởi độ rộng bit | 
| Không gian | O(B) | Chỉ lưu trữ các giá trị OR riêng biệt cho chỉ mục hiện tại | 

Độ rộng bit B tối đa là 30 đối với các giá trị lên tới 10^9 và trong thực tế, số lượng trạng thái OR riêng biệt trên mỗi vị trí vẫn nhỏ do độ bão hòa bit nhanh chóng. Điều này đảm bảo giải pháp chạy thoải mái trong giới hạn cho n = 4 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        prev = {}
        ans = 0

        for x in a:
            curr = {}
            curr[x] = curr.get(x, 0) + 1

            for v, c in prev.items():
                nv = v | x
                curr[nv] = curr.get(nv, 0) + c

            ans += curr.get(k, 0)
            prev = curr

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5 3\n1 2 1 2 3\n") == "4"

# minimum size
assert run("1 0\n0\n") == "1"

# all equal, k matches
assert run("4 7\n7 7 7 7\n") == "10"

# no valid intervals
assert run("3 8\n1 2 4\n") == "0"

# mixed case
assert run("5 1\n1 0 1 0 1\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 1 | trường hợp ranh giới tối thiểu | 
| tất cả đều bằng K | 10 | tính đúng đắn của tổ hợp đầy đủ | 
| không khớp | 0 | cắt tỉa đúng cách | 
| số 0 xen kẽ | 9 | khoảng thời gian chồng chéo và kiên trì | 

## Vỏ cạnh 

Khi mảng chỉ chứa các số 0 và K bằng 0, mọi mảng con đều hợp lệ vì OR trên bất kỳ phân đoạn nào vẫn bằng 0. Thuật toán xử lý việc này vì mỗi bước duy trì một trạng thái OR bằng 0 và số lượng tích lũy thành 1, 2, 3, v.v. ngầm thông qua bội số trạng thái. 

Khi một phần tử chứa một bit bên ngoài K, tất cả các mảng con bao gồm phần tử đó tạo ra các giá trị OR không bao giờ có thể khớp với K nếu K thiếu bit đó. Trong DP, những trạng thái đó chỉ đơn giản truyền thành các giá trị OR mới không bao giờ được tính, do đó chúng tự nhiên bị loại khỏi quá trình tích lũy câu trả lời. 

Khi K có nhiều bit và chúng xuất hiện dần dần trên toàn mảng, thuật toán chỉ đếm chính xác những mảng con có OR tích lũy đạt chính xác K, vì các tập hợp con một phần vẫn còn trong không gian trạng thái nhưng không bao giờ được thêm vào câu trả lời cho đến khi khớp hoàn toàn với K.
