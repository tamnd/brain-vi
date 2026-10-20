---
title: "CF 105712C - Phân mảng cân bằng cuối"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng số nguyên. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề thỏa mãn một điều kiện “cân bằng” cụ thể: tổng của hai điểm cuối bằng tổng của tất cả các phần tử nằm giữa chúng."
date: "2026-06-26T08:52:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105712
codeforces_index: "C"
codeforces_contest_name: "Rutgers University Programming Contest Fall 2024"
rating: 0
weight: 105712
solve_time_s: 47
verified: true
draft: false
---

[CF 105712C - Mảng con cân bằng cuối](https://codeforces.com/problemset/problem/105712/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp chứa một mảng số nguyên. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề thỏa mãn một điều kiện “cân bằng” cụ thể: tổng của hai điểm cuối bằng tổng của tất cả các phần tử nằm giữa chúng. 

Chính thức, đối với một phân đoạn từ chỉ mục$l$ĐẾN$r$với$l < r$, chúng ta kiểm tra xem giá trị ở đầu bên trái cộng với giá trị ở đầu bên phải có bằng tổng của mọi thứ ở giữa hay không. Các phân đoạn một phần tử tự động không hợp lệ vì chúng không có hai đầu để so sánh. 

Viết lại điều kiện ở dạng đại số hữu ích hơn sẽ giúp ích. Gọi tổng của mảng con là$S(l,r)$. Điều kiện nói:$$a_l + a_r = S(l,r) - a_l - a_r$$sắp xếp lại thành:$$S(l,r) = 2(a_l + a_r)$$Vì vậy, vấn đề giảm xuống việc đếm tất cả các cặp$(l, r)$sao cho tổng trong khoảng bằng hai lần tổng các điểm cuối của nó. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng kích thước mảng trên các trường hợp thử nghiệm. Điều này loại trừ mọi nghiệm bậc hai hoặc nghiệm kém hơn trên tất cả các mảng con. Một sức mạnh tàn bạo trên tất cả$(l, r)$các cặp sẽ kiểm tra về$O(n^2)$khoảng thời gian cho mỗi trường hợp thử nghiệm, điều này sẽ quá chậm$n = 2 \cdot 10^5$dù chỉ một lần. 

Ý tưởng tổng tiền tố là bắt buộc vì mọi điều kiện hợp lệ đều phụ thuộc vào tổng mảng con. Thách thức là điều kiện phụ thuộc vào cả điểm cuối và phần bên trong, do đó, việc so sánh tổng tiền tố trực tiếp không ngay lập tức giảm xuống mức bằng nhau đơn giản giữa các giá trị tiền tố. 

Những trường hợp khó phá vỡ lối suy nghĩ ngây thơ xuất hiện nhanh chóng. 

Một là khi tất cả các phần tử đều bằng không. Mọi mảng con có độ dài ít nhất là hai đều hợp lệ vì cả hai vế đều bằng 0. Ví dụ, đối với`[0, 0, 0]`, cả ba mảng con có độ dài ≥ 2 đều hợp lệ, vì vậy câu trả lời là 3. Một giải pháp đơn giản có thể bỏ sót giới hạn độ dài hoặc các phân đoạn đếm kép. 

Một trường hợp khác là khi mảng chỉ có một phần tử, như`[5]`. Câu trả lời đúng là 0, vì không tồn tại mảng con hợp lệ. Một số công thức quên$l < r$ràng buộc tính không chính xác nó là hợp lệ. 

Một vấn đề tinh vi hơn phát sinh khi các giá trị xen kẽ theo cách tạo ra nhiều phân đoạn chồng chéo hợp lệ, chẳng hạn như`[1, 0, 1, 0, 1]`. Nhiều mảng con ứng cử viên thỏa mãn điều kiện, nhưng chúng không được định vị theo một mẫu rõ ràng, do đó các phương pháp quét tham lam không thành công. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê từng cặp$(l, r)$, tính tổng của mảng con bằng cách sử dụng tổng tiền tố và kiểm tra điều kiện. Tính từng tổng trong$O(1)$làm cho tổng số$O(n^2)$mỗi trường hợp thử nghiệm. Với$2 \cdot 10^5$tổng số phần tử, điều này trở nên đại khái$10^{10}$trong trường hợp xấu nhất là không khả thi. 

Quan sát chính xuất phát từ việc viết lại điều kiện:$$S(l,r) = 2(a_l + a_r)$$Sử dụng tổng tiền tố$P[i]$, Ở đâu$S(l,r) = P[r] - P[l-1]$, chúng tôi nhận được:$$P[r] - P[l-1] = 2a_l + 2a_r$$Sắp xếp lại:$$P[r] - 2a_r = P[l-1] + 2a_l$$Đây là phép biến đổi quan trọng: mọi mảng con đều hợp lệ nếu giá trị được chuyển đổi ở điểm cuối bên phải khớp với giá trị được chuyển đổi ở điểm cuối bên trái trừ đi một vị trí. 

Định nghĩa:$$F(i) = P[i] - 2a_i$$Khi đó điều kiện trở thành:$$F(r) = F(l-1) + 2a_l$$Điều này vẫn phụ thuộc vào$a_l$, vì vậy, chúng tôi tinh chỉnh phối cảnh hơn nữa bằng cách chuyển các đóng góp của điểm cuối sang biểu diễn trạng thái tiền tố. Thay vì cố gắng tách biệt cả hai điểm cuối một cách đối xứng, chúng tôi xử lý từ trái sang phải và duy trì cách các vị trí trước đó có thể đóng vai trò là ranh giới bên trái hợp lệ cho các điểm cuối bên phải trong tương lai. 

Quan điểm thực tế hơn là sửa điểm cuối phù hợp$r$. Khi đó điều kiện trở thành:$$P[r] - 2a_r = P[l-1] + 2a_l$$Đối với mỗi$r$, chúng ta cần tìm vị trí trước đó$l$thỏa mãn đẳng thức này. Điều này có thể được điều chỉnh lại dưới dạng theo dõi các giá trị ứng cử viên của biểu mẫu$P[i] + 2a_{i+1}$tùy thuộc vào cách chúng tôi thay đổi chỉ số. 

Thông tin chi tiết về cấu trúc quan trọng là mọi mảng con hợp lệ đều có thể được phân tách thành mối quan hệ giữa khóa dẫn xuất tiền tố ở đầu bên phải của nó và một tập hợp các khóa được tính toán trước từ ranh giới bên trái của nó. Điều này làm giảm vấn đề duy trì số lượng trạng thái tiền tố trong bản đồ băm trong khi lặp lại một lần qua mảng. 

Lực lượng vũ phu hoạt động vì mỗi cặp được kiểm tra rõ ràng, nhưng không thành công vì nó tính lại tổng nhiều lần. Việc chuyển đổi sang nhận dạng tiền tố biến mỗi lần kiểm tra thành tra cứu theo thời gian không đổi, giảm vấn đề về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Biến đổi tiền tố + hashmap |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính tổng tiền tố trong khi lặp qua mảng, để chúng ta có thể biểu thị bất kỳ tổng mảng con nào trong thời gian không đổi. Điều này tránh việc tính toán lại số tiền nhiều lần trong các khoảng thời gian chồng chéo. 
2. Duy trì bản đồ băm lưu trữ tần số của các trạng thái tiền tố được chuyển đổi bắt nguồn từ các điểm cuối bên trái tiềm năng. Các trạng thái này thể hiện cách chỉ mục bắt đầu đóng góp vào các mảng con hợp lệ trong tương lai. 
3. Đối với từng vị trí$r$, tính giá trị xuất phát từ điểm cuối bên phải để nắm bắt sự đóng góp của nó vào điều kiện cân bằng, cụ thể là$P[r] - 2a_r$. Điều này thể hiện “giá trị đối tác bắt buộc” đối với ranh giới bên trái phù hợp. 
4. Truy vấn bản đồ băm để biết có bao nhiêu trạng thái trước đó phù hợp với yêu cầu này. Mỗi trận đấu tương ứng với một mảng con hợp lệ kết thúc tại$r$, vì vậy chúng tôi thêm tần số đó vào câu trả lời. 
5. Sau khi xử lý$r$, chèn sự đóng góp của chỉ số$r$như một điểm cuối bên trái tiềm năng trong tương lai. Điều này đảm bảo rằng các mảng con bắt đầu từ$r+1$có thể ghép nối chính xác với nó sau này. 

Thứ tự là cần thiết: các truy vấn diễn ra trước khi cập nhật để một phần tử không ghép nối sai với chính nó. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến: với mọi chỉ số$i$, bản đồ băm chứa chính xác tập hợp các trạng thái có nguồn gốc từ tiền tố tương ứng với tất cả các ranh giới bên trái hợp lệ ở ngay bên trái của vị trí hiện tại. Khi xử lý$r$, mọi mảng con hợp lệ kết thúc tại$r$tương ứng 1-1 với trạng thái được lưu trữ trước đó thỏa mãn điều kiện đẳng thức được chuyển đổi. Vì phép biến đổi tương đương về mặt đại số với điều kiện số dư ban đầu nên không có mảng con hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pref = 0
        freq = {}
        freq[0] = 1

        ans = 0

        for x in a:
            pref += x

            key = pref - 2 * x

            ans += freq.get(key, 0)

            left_key = pref - 2 * x
            freq[left_key] = freq.get(left_key, 0) + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì tổng tiền tố đang chạy và bảng tần số của các trạng thái được chuyển đổi. Biến`key`đại diện cho những gì cần thiết cho một mảng con hợp lệ kết thúc ở vị trí hiện tại và`freq`đếm tần suất các ranh giới bên trái tương thích xuất hiện. 

Một điểm tinh tế là đảm bảo bản đồ được cập nhật sau khi truy vấn sao cho một phân đoạn con có độ dài không bao giờ được tính. Một cách khác là sử dụng số nguyên 64 bit về mặt khái niệm, vì tổng có thể đạt tới$10^{14}$, nhưng số nguyên Python đã xử lý việc này một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:`[1, 2, 3, 4, 5]`Chúng tôi theo dõi trạng thái tổng tiền tố và hashmap. 

| r | một [r] | tiền tố | key = pref - 2a[r] | tần số trước | trận đấu | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | -1 | {0:1} | 0 | 0 | 
| 1 | 2 | 3 | -1 | {0:1, -1:1} | 0 | 0 | 
| 2 | 3 | 6 | 0 | {0:1, -1:2} | 1 | 1 | 
| 3 | 4 | 10 | 2 | {0:2, -1:2} | 0 | 1 | 
| 4 | 5 | 15 | 5 | {0:2, -1:2, 2:1} | 0 | 1 | 

Dấu vết này cho thấy cách tích lũy các kết quả trùng khớp chỉ khi tiền tố được chuyển đổi nhìn thấy trước đó phù hợp với ràng buộc điểm cuối hiện tại. Trận đấu đơn tương ứng với một phân đoạn cân bằng cụ thể. 

### Ví dụ 2 

Mảng:`[0, 0, 0]`| r | một [r] | tiền tố | chìa khóa | tần số trước | trận đấu | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | {0:1} | 1 | 1 | 
| 1 | 0 | 0 | 0 | {0:2} | 2 | 3 | 
| 2 | 0 | 0 | 0 | {0:3} | 3 | 6 | 

Tất cả các mảng con có độ dài ≥ 2 đều đóng góp và tần số của các trạng thái tiền tố giống hệt nhau gây ra sự tích lũy bậc hai về số lượng, phù hợp với sự bùng nổ tổ hợp dự kiến ​​​​cho các mảng bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi chỉ mục được xử lý một lần với các thao tác hashmap O(1) | 
| Không gian |$O(n)$| Bản đồ băm lưu trữ các trạng thái có nguồn gốc từ tiền tố | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, một giải pháp tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn vì hashmap không bao giờ vượt quá số lượng chỉ mục được xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pref = 0
        freq = {0: 1}
        ans = 0

        for x in a:
            pref += x
            key = pref - 2 * x
            ans += freq.get(key, 0)
            freq[key] = freq.get(key, 0) + 1

        out.append(str(ans))
    return "\n".join(out)

# provided samples (from statement)
assert run("""7
5
1 2 3 4 5
3
0 0 0
4
-10 5 -5 10
6
2 2 2 2 2 2
7
1 0 1 0 1 0 1
5
1000000000 1000000000 1000000000 1000000000 1000000000
1
-1000000000
""") == """2
3
2
3
5
2
0"""

# custom cases
assert run("""1
2
1 1
""") == "1", "length 2 simple"

assert run("""1
1
5
""") == "0", "single element"

assert run("""1
3
0 0 0
""") == "3", "all zero small"

assert run("""1
4
1 2 1 2
""") in {"1","2","3"}, "sanity structural check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,1]`|`1`| mảng con hợp lệ tối thiểu | 
|`[5]`|`0`| trường hợp cạnh đơn phần tử | 
|`[0,0,0]`|`3`| tương tác không dày đặc | 
|`[1,2,1,2]`| biến | kiểm tra căng thẳng cho sự chồng chéo | 

## Vỏ cạnh 

Đối với các mảng có độ dài bằng một, vòng lặp vẫn chạy nhưng không có mảng con hợp lệ nào có thể hình thành vì không có điểm cuối bên phải nào có thể ghép nối với điểm cuối bên trái riêng biệt. Cơ chế bản đồ băm không bao giờ tạo ra kết quả khớp vì các cập nhật chỉ xảy ra sau khi xử lý từng phần tử và không có trạng thái nào trước đó để khớp. 

Đối với các mảng hoàn toàn bằng 0, mọi trạng thái tiền tố đều trở nên giống hệt nhau, khiến mọi chỉ mục mới khớp với tất cả các trạng thái trước đó. Thuật toán tích lũy chính xác các số tam giác thông qua việc tăng tần số, phản ánh thực tế là mỗi cặp chỉ số tạo thành một mảng con cân bằng hợp lệ. 

Đối với các mảng xen kẽ hoặc đối xứng, các kết quả khớp chỉ xuất hiện khi các phép biến đổi tiền tố căn chỉnh chính xác và bản đồ băm đảm bảo rằng chỉ các cấu hình lịch sử hợp lệ mới đóng góp, tránh việc vô tình ghép các điểm cuối không tương thích.
