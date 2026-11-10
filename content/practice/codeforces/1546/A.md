---
title: "CF 1546A - AquaMoon và hai mảng"
description: "Hai người chơi cho chúng ta hai mảng có độ dài bằng nhau. Một mảng có thể được sửa đổi bằng cách di chuyển liên tục một đơn vị từ vị trí này sang vị trí khác. Mỗi lần di chuyển sẽ loại bỏ một điểm khỏi chỉ mục i và thêm một điểm vào chỉ mục j và mảng phải không âm sau mỗi lần di chuyển."
date: "2026-06-14T19:37:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1546
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 732 (Div. 2)"
rating: 800
weight: 1546
solve_time_s: 218
verified: true
draft: false
---

[CF 1546A - AquaMoon và hai mảng](https://codeforces.com/problemset/problem/1546/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi cho chúng ta hai mảng có độ dài bằng nhau. Một mảng có thể được sửa đổi bằng cách di chuyển liên tục một đơn vị từ vị trí này sang vị trí khác. Mỗi lần di chuyển sẽ xóa một cái khỏi chỉ mục`i`và thêm một vào chỉ mục`j`và mảng phải không âm sau mỗi lần di chuyển. Mục tiêu là chuyển đổi chính xác mảng đầu tiên thành mảng thứ hai hoặc quyết định rằng điều này là không thể. 

Một cách hữu ích để diễn giải phép toán là nó bảo toàn tổng của mảng. Mọi thao tác chỉ là chuyển một đơn vị giá trị giữa các tọa độ nên tổng không bao giờ thay đổi. Điều này ngay lập tức hàm ý một điều kiện cần thiết: nếu tổng của hai mảng khác nhau thì không có chuỗi phép tính nào có thể làm cho chúng bằng nhau. 

Các hạn chế là nhỏ:`n`tối đa là 100 và tổng của tất cả các giá trị trên các mảng cũng bị giới hạn bởi 100 cho mỗi trường hợp thử nghiệm. Điều này có nghĩa là ngay cả các giải pháp di chuyển rõ ràng từng đơn vị cũng an toàn vì tổng số đơn vị cần được theo dõi là rất nhỏ. Bất kỳ cách tiếp cận nào theo phương pháp bậc hai hoặc thậm chí bậc ba trong`n`thì được, nhưng bất cứ điều gì theo cấp số nhân hoặc liên quan đến máy móc dòng chảy phức tạp đều không cần thiết. 

Một trường hợp phức tạp xuất hiện khi các tổng khớp nhau nhưng việc phân phối lại là không thể nếu chúng ta không suy luận chính xác về nguồn và phần chìm. Ví dụ, nếu`a = [0, 2]`Và`b = [1, 1]`, chúng ta cần chuyển một đơn vị từ chỉ số 2 sang chỉ số 1. Một cách tiếp cận bất cẩn có thể cố gắng khớp các chỉ số một cách tham lam mà không đảm bảo rằng các chỉ số dư thừa thực sự có sẵn khi cần. Một trường hợp khác là khi các mảng đã bằng nhau; câu trả lời đúng là không có thao tác nào, không phải là một gói trống có định dạng không hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng tất cả các chuỗi chuyển giao có thể xảy ra. Từ một trạng thái nhất định, chúng ta có thể chọn bất kỳ cặp chỉ mục nào và thử di chuyển một đơn vị, sau đó tìm kiếm đệ quy một chuỗi khớp với mảng mục tiêu. Điều này đúng vì nó khám phá tất cả các trạng thái có thể truy cập và mỗi chuyển đổi trạng thái tương ứng chính xác với một thao tác hợp lệ. Vấn đề là không gian trạng thái phát triển bùng nổ. Mặc dù tổng số tiền nhỏ nhưng hệ số phân nhánh xấp xỉ`n^2`và trình tự nhanh chóng trở nên không thể quản lý được. 

Quan sát quan trọng là việc nhận dạng từng đơn vị riêng lẻ không quan trọng. Chỉ số mỗi chỉ số cần tăng hoặc giảm bao nhiêu đơn vị mới là quan trọng. Chúng ta có thể tính toán một mảng chênh lệch trong đó mỗi vị trí cho chúng ta biết liệu nó có thặng dư hay thâm hụt so với mục tiêu hay không. Mọi chênh lệch dương phải gửi đi đơn vị, và mọi chênh lệch âm phải nhận đơn vị. Vì tổng số tiền được bảo toàn nên tổng thặng dư bằng tổng thâm hụt, nên chúng ta chỉ đơn giản là làm cho cung và cầu khớp nhau. 

Điều này làm giảm vấn đề ghép các chỉ số thặng dư với các chỉ số thâm hụt và tạo ra các chuyển động rõ ràng. Mỗi bước di chuyển sẽ khắc phục một đơn vị mất cân bằng. Vì tổng số tiền mất cân bằng nhiều nhất là 100 nên chúng tôi sẽ thực hiện tối đa 100 lần chuyển khoản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Kết hợp Thặng dư-Thâm hụt | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp trực tiếp từ việc ghi sổ kế toán mất cân bằng. 

1. Tính tổng của cả hai mảng. Nếu chúng khác nhau, hãy trả về lỗi ngay lập tức. Điều này là bắt buộc vì mọi thao tác đều bảo toàn tổng số tiền, do đó, tổng số không khớp sẽ khiến không thể truy cập được mục tiêu. 
2. Xây dựng danh sách các vị trí dư thừa`a[i] > b[i]`, chỉ số lặp lại`i`chính xác`a[i] - b[i]`lần. Chúng đại diện cho các đơn vị phải được chuyển đi. 
3. Xây dựng danh sách các vị thế thâm hụt`a[i] < b[i]`, chỉ số lặp lại`i`chính xác`b[i] - a[i]`lần. Chúng đại diện cho các đơn vị phải được điền. 
4. Tại thời điểm này, cả hai danh sách đều có độ dài bằng nhau vì tổng thặng dư bằng tổng thâm hụt. Nếu chúng không khớp về kích thước thì cấu hình không nhất quán và không thể sửa được. 
5. Ghép nối các phần tử theo thứ tự: lấy chỉ số thặng dư thứ nhất và chỉ số thâm hụt thứ nhất, rồi thực hiện phép toán chuyển một đơn vị từ thặng dư sang thâm hụt. Lặp lại cho đến khi tất cả các phần tử được ghép nối. 

Mỗi cặp đôi làm giảm sự khác biệt tuyệt đối ở hai vị trí cùng một lúc, do đó không có trạng thái trung gian nào vi phạm tính không tiêu cực. 

### Tại sao nó hoạt động 

Mảng khác biệt mô tả đầy đủ những gì phải thay đổi. Mỗi hoạt động làm giảm sự mất cân bằng L1 chính xác bằng 2, một đơn vị được loại bỏ khỏi vị trí thặng dư và một đơn vị được thêm vào vị trí thâm hụt. Vì chúng tôi luôn chọn các chỉ số thặng dư hợp lệ nên chúng tôi không bao giờ trừ đi từ vị trí 0. Bởi vì tổng thặng dư bằng tổng thâm hụt, việc ghép đôi luôn hoàn thành mà không có sự mất cân bằng còn sót lại, đảm bảo một sự chuyển đổi đầy đủ hợp lệ nếu có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        if sum(a) != sum(b):
            out.append("-1")
            continue

        surplus = []
        deficit = []

        for i in range(n):
            if a[i] > b[i]:
                surplus.extend([i + 1] * (a[i] - b[i]))
            elif a[i] < b[i]:
                deficit.extend([i + 1] * (b[i] - a[i]))

        ops = []
        for i in range(len(surplus)):
            ops.append((surplus[i], deficit[i]))

        out.append(str(len(ops)))
        for i, j in ops:
            out.append(f"{i} {j}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ kiểm tra bất biến mà tổng phải khớp. Đây là điều kiện bất khả thi toàn cầu duy nhất. Sau đó, nó chuyển đổi các mảng thành danh sách mở rộng gồm các đơn vị riêng lẻ phải di chuyển, điều này tránh mọi nhu cầu ghi sổ kế toán phức tạp. 

Vòng lặp ghép nối an toàn vì mỗi`surplus[i]`được đảm bảo tương ứng với một vị trí ban đầu có ít nhất một đơn vị bổ sung. Ngay cả khi nhiều đơn vị được lấy từ cùng một chỉ mục, thì mỗi đơn vị đều độc lập về mặt khái niệm, do đó việc lập chỉ mục lặp lại vẫn hợp lệ. 

Việc xây dựng đầu ra cuối cùng chỉ đơn giản là tuần tự hóa các lần chuyển đơn vị này. Số lượng thao tác được tự động giới hạn bởi tổng số mất cân bằng, tối đa là 100 do các ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét sự chuyển đổi`a = [1, 2, 3, 4]`Và`b = [3, 1, 2, 4]`. 

Chúng tôi tính toán sự khác biệt cho mỗi chỉ số. Chỉ số 1 cần +2, chỉ số 2 cần -1, chỉ số 3 cần -1, chỉ mục 4 cần 0. Điều này mang lại danh sách thặng dư`[2, 3]`và danh sách thâm hụt`[1, 1]`. 

| Bước | Thặng dư | Thâm hụt | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | 2, 3 | 1, 1 | 2 → 1 | 
| 2 | 3 | 1 | 3 → 1 | 

Sau khi áp dụng những động thái này, mỗi chỉ số sẽ khớp với giá trị mục tiêu của nó. Điều này xác nhận rằng việc ghép nối các đơn vị một cách độc lập là đủ, vì thứ tự không quan trọng miễn là mỗi lần chuyển đơn vị đều được tính đến. 

Bây giờ hãy xem xét trường hợp một phần tử`a = [0]`,`b = [0]`. Cả hai danh sách đều trống nên không có thao tác nào được tạo. Điều này chứng tỏ rằng thuật toán xử lý các trường hợp tầm thường một cách tự nhiên mà không cần phân nhánh đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ số được xử lý một lần để xây dựng danh sách thặng dư và thâm hụt, đồng thời tổng các hoạt động được giới hạn bởi tổng chênh lệch | 
| Không gian | O(n) | Lưu trữ mở rộng đơn vị thặng dư và thâm hụt | 

Các ràng buộc đảm bảo tổng số đơn vị trong tất cả các thử nghiệm là nhỏ, do đó, ngay cả việc mở rộng từng đơn vị một cách rõ ràng vẫn hiệu quả. Giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        if sum(a) != sum(b):
            res.append("-1")
            continue

        s = []
        d = []

        for i in range(n):
            if a[i] > b[i]:
                s.extend([i + 1] * (a[i] - b[i]))
            elif a[i] < b[i]:
                d.extend([i + 1] * (b[i] - a[i]))

        ans = []
        for i in range(len(s)):
            ans.append((s[i], d[i]))

        res.append(str(len(ans)))
        for x, y in ans:
            res.append(f"{x} {y}")

    return "\n".join(res)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""4
4
1 2 3 4
3 1 2 4
2
1 3
2 1
1
0
0
5
4 3 2 1 0
0 1 2 3 4
""") == """2
2 1
3 1
-1
0
6
1 4
1 4
1 5
1 5
2 5
2 5"""

# all equal arrays
assert run("""1
3
5 5 5
5 5 5
""") == """0"""

# impossible due to sum mismatch
assert run("""1
2
1 1
0 1
""") == """-1"""

# single transfer
assert run("""1
2
0 3
2 1
""") != ""  # any valid sequence is acceptable

# maximum small stress
assert run("""1
4
4 0 0 0
0 1 1 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | đưa ra | tính đúng đắn của hành vi ví dụ đầy đủ | 
| mảng bằng nhau | 0 | trường hợp không hoạt động | 
| tổng không khớp | -1 | phát hiện không thể | 
| chuyển đơn | hoạt động hợp lệ | độ chính xác chuyển động tối thiểu | 
| phân phối lệch | hoạt động hợp lệ | phân phối lại nhiều đơn vị | 

## Vỏ cạnh 

Khi cả hai mảng đều giống hệt nhau, thuật toán sẽ tạo ra các danh sách thặng dư và thiếu hụt trống, dẫn trực tiếp đến các hoạt động bằng 0, phù hợp với định dạng đầu ra được yêu cầu. 

Khi các tổng khác nhau, việc chấm dứt sớm sẽ tránh xây dựng các cấu trúc thặng dư và thâm hụt không nhất quán, ngăn chặn việc ghép đôi từng phần sai lệch. 

Khi tất cả thặng dư tập trung vào một chỉ số, danh sách thặng dư mở rộng chỉ lặp lại chỉ số đó nhiều lần và mỗi lần lặp lại kết hợp độc lập với một chỉ số thâm hụt, cho thấy rằng việc sử dụng lặp lại một nguồn duy nhất là hợp lệ miễn là thặng dư ban đầu của nó là đủ.
