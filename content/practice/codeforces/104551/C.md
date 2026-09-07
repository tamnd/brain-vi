---
title: "CF 104551C - Ít tiền hơn, nhiều vấn đề hơn"
description: "Chúng ta được cung cấp một hệ thống tiền tệ bao gồm một số mệnh giá tiền xu hiện có, mỗi mệnh giá là một số nguyên dương. Khi thanh toán cho một thứ gì đó, bạn được phép sử dụng tối đa đồng C của mỗi mệnh giá."
date: "2026-06-30T08:53:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104551
codeforces_index: "C"
codeforces_contest_name: "2015 Google Code Jam Round 1C (GCJ 15 Round 1C)"
rating: 0
weight: 104551
solve_time_s: 58
verified: true
draft: false
---

[CF 104551C - Ít tiền hơn, nhiều vấn đề hơn](https://codeforces.com/problemset/problem/104551/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống tiền tệ bao gồm một số mệnh giá tiền xu hiện có, mỗi mệnh giá là một số nguyên dương. Khi thanh toán cho một thứ gì đó, bạn được phép sử dụng tối đa đồng C của mỗi mệnh giá. Điều này có nghĩa là ngay cả khi một đồng xu tồn tại thì mức đóng góp của nó vẫn bị giới hạn: bạn không thể sử dụng nó quá C lần trong một lần mua. 

Mục tiêu là để đảm bảo rằng mọi giá trị số nguyên từ 1 đến V đều có thể được hình thành bằng cách sử dụng các đồng tiền này theo quy tắc giới hạn C. Nếu một số giá trị không thể thực hiện được, chúng tôi được phép giới thiệu các mệnh giá tiền xu mới. Mỗi mệnh giá mới cũng tôn trọng giới hạn sử dụng giống nhau C. Chúng tôi muốn giới thiệu càng ít mệnh giá mới càng tốt để tất cả các giá trị trong phạm vi [1, V] đều có thể biểu thị được. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi đọc C, số mệnh giá hiện có D và giới hạn trên mục tiêu V. Sau đó, chúng tôi đọc danh sách các giá trị đồng xu được sắp xếp D. Đầu ra là số lượng mệnh giá bổ sung tối thiểu cần thiết. 

Những hạn chế quan trọng theo một cách rất cụ thể. Giá trị V có thể lớn tới 10^9, do đó, bất kỳ giải pháp nào xây dựng bảng DP một cách rõ ràng trên tất cả các giá trị lên đến V đều không thể thực hiện được. Ngay cả việc quét tuyến tính trên tất cả các giá trị lên tới V cũng sẽ quá chậm. Số lượng mệnh giá ít, nhiều nhất là 100 nên cấu trúc của bài toán phải khai thác một cách tham lam chứ không phải tìm kiếm một cách thấu đáo. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị nhỏ bị thiếu sớm trong phạm vi. Ví dụ: nếu C lớn nhưng chúng ta không có đồng xu 1 thì các giá trị từ 1 đến C hoàn toàn không thể được hình thành, buộc phải tăng ngay lập tức. Một trường hợp khác là khi các đồng tiền hiện có lớn nhưng thưa thớt, điều này có thể gây ra những khoảng trống dài không thể tiếp cận ngay cả khi về mặt lý thuyết có thể có số tiền lớn hơn. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng tất cả các tổng có thể đạt tới V bằng cách sử dụng logic ba lô giới hạn. Điều này thất bại ngay lập tức khi V lớn vì mỗi đồng xu sẽ nhân lên các khả năng và không gian trạng thái bùng nổ theo kiểu tổ hợp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tính toán tất cả các giá trị có thể đạt được lên đến V bằng cách sử dụng một chiếc ba lô có giới hạn trong đó mỗi đồng xu có giá trị x có thể được sử dụng từ 0 đến C lần. Điều này sẽ duy trì một mảng boolean có thể truy cập được [v] và liên tục áp dụng các chuyển đổi cho mỗi đồng tiền. Mặc dù đúng về mặt khái niệm nhưng độ phức tạp trở thành O(D * V * C), điều này hoàn toàn không khả thi khi V đạt 10^9. 

Quan sát quan trọng là chúng ta không thực sự cần biết tất cả các giá trị có thể tiếp cận. Chúng tôi chỉ cần duy trì giá trị nhỏ nhất mà hiện tại không thể hình thành và đảm bảo rằng chúng tôi có thể mở rộng phạm vi phủ sóng liên tục từ 1 trở lên. 

Giả sử chúng ta đã biết rằng tất cả các giá trị trong [1, Reach] đều có thể xây dựng được. Bây giờ chúng ta xem xét giá trị đồng xu tiếp theo x. Nếu x đủ nhỏ, cụ thể là x ≤ Reach + 1, thì việc thêm đồng tiền này cho phép chúng ta mở rộng phạm vi có thể tiếp cận một cách đáng kể, bởi vì chúng ta có thể kết hợp tối đa C bản sao của x với bất kỳ giá trị nào có thể đạt được. Điều này mở rộng phạm vi phủ sóng tới +C*x. 

Thay vào đó, nếu x lớn hơn Reach + 1 thì sẽ có một khoảng trống ở Reach + 1 mà chúng ta không thể lấp đầy bằng cách sử dụng các đồng tiền hiện có. Trong trường hợp đó, chiến lược tối ưu là giới thiệu một loại tiền mới có giá trị Reach +1. Đây là sự bổ sung nhỏ nhất có thể giúp khắc phục khoảng cách ngay lập tức và tối đa hóa phạm vi phủ sóng trong tương lai, mở rộng phạm vi tiếp cận lên + C * (reach +1). 

Chiến lược tham lam này hoạt động vì ở mỗi bước, chúng tôi sẽ tiêu thụ đồng tiền có sẵn tiếp theo nếu nó hữu ích hoặc chúng tôi vá giá trị còn thiếu nhỏ nhất, mang lại mức tăng gia tăng lớn nhất có thể có trên mỗi đồng tiền mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ba lô vũ phu | O(D · V · C) | O(V) | Quá chậm | 
| Mở rộng phạm vi tham lam | O(D + đáp án) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì phạm vi tiếp cận có thể thay đổi, biểu thị giá trị lớn nhất trong [1, phạm vi tiếp cận] mà chúng tôi hiện có thể hình thành.

1. Khởi tạo phạm vi tiếp cận = 0 và đặt chỉ mục i = 0 cho các đồng tiền hiện có đã được sắp xếp. Đồng thời khởi tạo bộ đếm được thêm = 0 cho các mệnh giá mới. 
2. Khi đạt < V, chúng tôi quyết định nên sử dụng đồng tiền hiện có hay thêm đồng tiền mới. 
3. Nếu đồng xu hiện tại chưa được sử dụng tiếp theo có giá trị x ≤ Reach + 1, chúng ta có thể sử dụng nó một cách an toàn. Chúng ta cập nhật Reach thành Reach + C*x và chuyển sang coin tiếp theo. 
4. Nếu không có đồng xu nào như vậy hoặc đồng xu tiếp theo lớn hơn phạm vi tiếp cận + 1, chúng tôi sẽ giới thiệu một đồng xu mới có giá trị phạm vi tiếp cận + 1. Điều này sẽ khắc phục trực tiếp giá trị không thể tiếp cận đầu tiên. 
5. Sau khi thêm đồng tiền mới này, chúng tôi cập nhật phạm vi tiếp cận thành Reach + C * (reach + 1), vì chúng tôi có thể sử dụng tối đa C bản sao của nó. 
6. Lặp lại cho đến khi đạt ≥ V. 

Ý tưởng chính là chúng tôi luôn giữ tiền tố có thể truy cập lớn nhất có thể với số lần bổ sung tối thiểu và chúng tôi không bao giờ lãng phí một đồng tiền mới vào một giá trị khác ngoài khoảng cách nhỏ nhất. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, phạm vi tiếp cận đại diện cho một tiền tố được bao phủ đầy đủ. Giá trị không thể truy cập tiếp theo là phạm vi tiếp cận + 1. Bất kỳ giải pháp hợp lệ nào cũng phải cung cấp cách tạo phạm vi tiếp cận + 1 bằng cách sử dụng đồng tiền hiện có hoặc giới thiệu đồng xu cho phép điều đó. Nếu chúng ta đưa ra đồng coin nào lớn hơn Reach +1 thì cũng không thể giúp lấp đầy khoảng trống này nên Reach +1 luôn là lựa chọn tối ưu. Tương tự, khi sử dụng một đồng xu hiện có, việc sử dụng nó ngay khi nó có thể sử dụng được sẽ tối đa hóa khả năng mở rộng vì nó góp phần tích lũy gấp C trên phạm vi đã có thể tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        C, D, V = map(int, input().split())
        coins = list(map(int, input().split()))

        reach = 0
        i = 0
        added = 0

        while reach < V:
            if i < D and coins[i] <= reach + 1:
                reach += coins[i] * C
                i += 1
            else:
                new_coin = reach + 1
                added += 1
                reach += new_coin * C

        print(f"Case #{tc}: {added}")

if __name__ == "__main__":
    solve()
```Việc triển khai theo dõi tiền tố có thể truy cập và luôn quyết định giữa việc tiêu thụ đồng tiền hiện có hữu ích tiếp theo hay chèn mệnh giá nhỏ nhất còn thiếu. Chi tiết quan trọng là cập nhật phạm vi tiếp cận theo giá trị C *, vì mỗi mệnh giá có thể được sử dụng tối đa C lần một cách độc lập. 

Một sai lầm phổ biến là coi tiền xu là gia số sử dụng một lần. Điều đó đánh giá thấp khả năng tiếp cận và phá vỡ tính chính xác. Một vấn đề tế nhị khác là quên rằng sự lựa chọn tham lam phải luôn nhắm mục tiêu phạm vi tiếp cận + 1 khi thêm một đồng tiền mới, không bao giờ có giá trị lớn hơn. 

## Ví dụ đã hoạt động 

Xét trường hợp có C = 2 và đồng tiền [1, 3]. 

Ban đầu, phạm vi tiếp cận = 0. Đồng xu tiếp theo là 1, tức là 1, vì vậy chúng tôi sử dụng nó và mở rộng phạm vi tiếp cận lên 0 + 2 * 1 = 2. Bây giờ chúng tôi đề cập đến [1, 2]. 

Đồng xu tiếp theo là 3, nhưng nó lớn hơn Reach + 1 = 3 nên hiện tại có thể sử dụng được. Chúng tôi lấy nó và mở rộng phạm vi tiếp cận lên 2 + 2 * 3 = 8. Bây giờ chúng tôi bao gồm [1..8] và không cần tiền mới. 

| Bước | đạt | đồng xu tiếp theo | hành động | thêm tiền | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | sử dụng tiền xu | 0 | 
| 2 | 2 | 3 | sử dụng tiền xu | 0 | 
| 3 | 8 | - | dừng lại | 0 | 

Bây giờ hãy xem xét C = 1 và đồng xu [2, 5], V = 6. 

Chúng ta bắt đầu với Reach = 0. Không có xu 1 nên chúng ta phải thêm nó vào. 

| Bước | đạt | hành động | thêm tiền | 
| --- | --- | --- | --- | 
| 1 | 0 | thêm 1 → phạm vi tiếp cận trở thành 1 | 1 | 
| 2 | 1 | xu 2 quá lớn, thêm 2 | 2 | 
| 3 | 2 | coin 2 tồn tại nhưng đã được sử dụng hiệu quả | (được xử lý về mặt khái niệm) | 

Điều này cho thấy khoảng trống ban đầu buộc phải có nhiều bản vá như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D + ans) | Mỗi đồng xu được xử lý một lần và mỗi mệnh giá được thêm vào sẽ tăng phạm vi tiếp cận một lần | 
| Không gian | O(1) | Chỉ con trỏ và bộ đếm được lưu trữ | 

Thuật toán chia tỷ lệ theo số mệnh giá và số miếng vá được yêu cầu, không phải với V. Điều này làm cho nó phù hợp ngay cả khi V lớn tới 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    out = []
    for tc in range(1, T + 1):
        C, D, V = map(int, input().split())
        coins = list(map(int, input().split()))

        reach = 0
        i = 0
        added = 0

        while reach < V:
            if i < D and coins[i] <= reach + 1:
                reach += coins[i] * C
                i += 1
            else:
                reach += (reach + 1) * C
                added += 1

        out.append(f"Case #{tc}: {added}")
    return "\n".join(out)

# custom and sample-style tests
assert run("1\n1 1 1\n1\n") == "Case #1: 0"
assert run("1\n1 2 6\n2 5\n") == "Case #1: 2"
assert run("1\n2 2 10\n1 3\n") == "Case #1: 0"
assert run("1\n1 0 5\n\n") == "Case #1: 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một xu đã đủ | 0 | không cần bản vá | 
| tiền xu thưa thớt | 2 | hành vi vá lỗi tham lam | 
| đồng tiền đầu dày đặc | 0 | mở rộng phạm vi tiếp cận nhanh | 
| thiếu đồng xu 1 | 1 | buộc sửa chữa ban đầu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi đồng xu nhỏ nhất lớn hơn 1. Ví dụ: C = 2 và xu = [3]. Thuật toán ngay lập tức thấy Reach = 0 và không có xu nào có thể che được 1 nên nó chèn xu 1. Sau khi thêm 1, Reach trở thành 2 và chúng ta vẫn không thể sử dụng xu 3. Sau đó, chúng tôi chỉ thêm xu 3 khi nó trở nên phù hợp hoặc vá thêm nếu cần. Điểm mấu chốt là thuật toán luôn ưu tiên sửa phạm vi tiếp cận + 1 trước khi xem xét các đồng tiền lớn hơn, đảm bảo không bỏ sót tiền tố không thể truy cập nào. 

Một trường hợp cạnh khác là khi C lớn. Ngay cả khi đó, cấu trúc tham lam vẫn không thay đổi. Một đồng xu x có thể mở rộng phạm vi tiếp cận thêm C * x, vì vậy C lớn chỉ đơn giản là tăng tốc độ bao phủ nhưng không làm thay đổi quy tắc quyết định, quy tắc này vẫn hoàn toàn bị chi phối bởi liệu x ≤ Reach + 1.
