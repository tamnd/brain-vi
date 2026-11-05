---
title: "CF 1208B - Tính độc đáo"
description: "Chúng ta được cung cấp một dãy số và chúng ta được phép xóa một khối liên tục khỏi nó hoặc không xóa gì cả. Sau lần xóa duy nhất này, tất cả các phần tử còn lại phải khác nhau."
date: "2026-06-15T17:52:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1208
codeforces_index: "B"
codeforces_contest_name: "Manthan, Codefest 19 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1500
weight: 1208
solve_time_s: 229
verified: true
draft: false
---

[CF 1208B - Tính duy nhất](https://codeforces.com/problemset/problem/1208/B) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, vũ phu, triển khai, hai con trỏ 
**Thời gian giải:** 3 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và chúng ta được phép xóa một khối liên tục khỏi nó hoặc không xóa gì cả. Sau lần xóa duy nhất này, tất cả các phần tử còn lại phải khác nhau. 

Nhiệm vụ là chọn một mảng con cần loại bỏ sao cho mảng cuối cùng không có giá trị lặp lại và trong số tất cả các lựa chọn hợp lệ, chúng tôi muốn phân đoạn bị loại bỏ ngắn nhất có thể. Nếu mảng không có bản sao thì câu trả lời là 0 vì chúng ta không cần xóa bất cứ thứ gì. 

Hạn chế chính ở đây là độ dài mảng tối đa là 2000. Điều này ngay lập tức gợi ý rằng các giải pháp bậc hai hoặc thậm chí bậc ba đều có thể chấp nhận được nếu được thực hiện cẩn thận, trong khi bất kỳ điều gì tệ hơn$O(n^2)$rủi ro trở thành ranh giới nhưng vẫn có khả năng chấp nhận được. Tuy nhiên, cấu trúc của bài toán gợi ý rõ ràng rằng chúng ta nên cố gắng tránh kiểm tra tất cả các mảng con có thể có theo cách được tính toán lại đầy đủ. 

Một cách giải thích ngây thơ sẽ là: thử từng cặp$(l, r)$, xóa phân đoạn đó, sau đó kiểm tra xem các phần tử còn lại có khác biệt không. Chi phí riêng của việc kiểm tra đó$O(n)$, và có$O(n^2)$mảng con, đưa ra$O(n^3)$, điều này là không cần thiết nhưng vẫn có thể vượt qua Python được tối ưu hóa dưới 2000. Tuy nhiên, chúng tôi có thể làm tốt hơn. 

Trường hợp cạnh tinh tế xuất hiện khi các bản sao đã được tách ra xa nhau. Ví dụ: trong một mảng như$[1, 2, 3, 1]$, việc xóa hợp lệ duy nhất phải loại bỏ ít nhất một lần xuất hiện của giá trị trùng lặp. Một cách tiếp cận bất cẩn chỉ kiểm tra tiền tố hoặc hậu tố có thể bỏ lỡ các giải pháp tối ưu trong đó việc loại bỏ nằm hoàn toàn ở giữa. 

Một trường hợp cạnh quan trọng khác là khi tất cả các phần tử đều giống hệt nhau. Ví dụ$[5, 5, 5, 5]$, điều tốt nhất chúng ta có thể làm là giữ lại nhiều nhất một phần tử, vì vậy chúng ta phải loại bỏ$n-1$các phần tử và phân đoạn tối ưu có thể ở bất kỳ đâu ngoại trừ việc để lại một phần tử duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực bắt đầu bằng việc chọn phân khúc$[l, r]$để loại bỏ. Sau khi xóa, chúng ta nối tiền tố$[1, l-1]$và hậu tố$[r+1, n]$, sau đó kiểm tra xem mảng kết hợp này có tất cả các giá trị riêng biệt hay không. 

Điều này hiệu quả vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, việc tính lại tính duy nhất từ ​​đầu cho mọi$(l, r)$cặp yêu cầu quét tối đa$n$các yếu tố, dẫn đến$O(n^3)$tổng số hoạt động trong trường hợp xấu nhất. Với$n = 2000$, điều này là quá chậm nếu được thực hiện một cách đơn giản. 

Quan sát quan trọng là khi chúng tôi sửa ranh giới bên trái của tiền tố được giữ lại, chúng tôi có thể mở rộng ranh giới bên phải của hậu tố một cách tham lam trong khi vẫn đảm bảo không có bản sao nào được đưa vào. Thay vì tính toán lại tính hợp lệ từ đầu cho mỗi phân đoạn bị xóa, chúng tôi xử lý giải pháp bằng cách chọn tách mảng thành tiền tố và hậu tố được giữ nguyên. Đoạn bị loại bỏ chính xác là đoạn nằm giữa chúng. 

Vì vậy, chúng tôi lặp lại xem chúng tôi giữ lại bao nhiêu phần tử ở phía bên trái, duy trì những giá trị nào xuất hiện ở đó và sau đó tìm hậu tố hợp lệ tối đa bắt đầu từ bên phải không đưa ra các bản sao với tiền tố. Điều này biến vấn đề thành sự tương tác kiểu hai con trỏ giữa tính hợp lệ của tiền tố và hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Quét tiền tố-hậu tố hai con trỏ | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn chọn tiền tố chúng tôi giữ và hậu tố chúng tôi giữ lại, đồng thời xóa mọi thứ ở giữa. Mục tiêu là tối đa hóa kích thước của các phần tử được lưu giữ trong khi vẫn đảm bảo tất cả các giá trị được lưu giữ là khác biệt. Việc giảm thiểu phân đoạn bị loại bỏ tương đương với việc tối đa hóa tiền tố và hậu tố được giữ lại. 

1. Cố định ranh giới bên trái$i$, nghĩa là chúng tôi giữ tiền tố$a_1 \dots a_i$. Chúng tôi đảm bảo tiền tố này không trùng lặp bằng cách theo dõi các giá trị đã thấy. Nếu xuất hiện bản sao, chúng tôi sẽ dừng lại vì việc mở rộng thêm là không hợp lệ. 
2. Đối với tiền tố cố định này, chúng tôi cố gắng chọn một hậu tố bắt đầu từ cuối mảng. Chúng tôi duy trì một tập hợp các giá trị đã được sử dụng trong tiền tố và mở rộng một con trỏ$j$từ$n$lạc hậu, chỉ bao gồm các phần tử không có trong bộ tiền tố và không trùng lặp trong chính hậu tố đó. 
3. Khi chúng ta di chuyển$j$sang trái, chúng tôi duy trì một tập hợp các phần tử hậu tố. Nếu một giá trị xung đột với tiền tố hoặc lặp lại ở hậu tố, chúng tôi sẽ ngừng mở rộng. 
4. Đối với mỗi tiền tố hợp lệ, chúng tôi tính toán hậu tố tốt nhất có thể và rút ra tổng độ dài được giữ. Đoạn bị xóa là khoảng cách giữa chúng nên chúng tôi cập nhật kích thước bị xóa tối thiểu. 
5. Chúng tôi lặp lại điều này cho tất cả các điểm cuối tiền tố hợp lệ. 

Ý tưởng tinh tế là khi tiền tố được cố định, hậu tố sẽ được tối đa hóa một cách tham lam từ bên phải, bởi vì việc lấy bất kỳ phần tử hậu tố nào sau đó chỉ làm giảm tính tự do và không bao giờ giúp ích cho tính hợp lệ trong tương lai. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bất kỳ mảng cuối cùng hợp lệ nào cũng có thể được biểu diễn dưới dạng tiền tố và hậu tố rời rạc và khác biệt riêng lẻ, đồng thời cũng tách biệt lẫn nhau về các giá trị. Đối với bất kỳ giải pháp tối ưu nào, phần tiền tố phải tương ứng với một số tiền tố của mảng ban đầu (sau khi chọn nơi bắt đầu loại bỏ) và tương tự, hậu tố tương ứng với hậu tố của mảng ban đầu. Bằng cách liệt kê tất cả các điểm cuối tiền tố và mở rộng các hậu tố một cách tham lam, chúng tôi đảm bảo rằng mọi phân chia tối ưu đều được xem xét ở một trong các lần lặp. Phần mở rộng hậu tố tham lam là an toàn vì một khi phần tử hậu tố bị loại trừ do trùng lặp, việc bao gồm nhiều phần tử trước đó trong hậu tố sẽ không thể khôi phục tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = n

    # try all prefix ends
    for i in range(n + 1):
        seen = set()
        ok = True

        # build prefix [0..i-1]
        for k in range(i):
            if a[k] in seen:
                ok = False
                break
            seen.add(a[k])

        if not ok:
            break

        # now greedily take suffix from right
        used_suffix = set()
        j = n

        while j > i:
            if a[j - 1] in seen or a[j - 1] in used_suffix:
                break
            used_suffix.add(a[j - 1])
            j -= 1

        kept = i + (n - j)
        ans = min(ans, n - kept)

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ thử mọi độ dài tiền tố. Đối với mỗi tiền tố, nó kiểm tra rõ ràng rằng tiền tố đó không chứa bản sao. Điều này là cần thiết vì một khi tiền tố trở nên không hợp lệ, việc mở rộng nó thêm chỉ khiến tình hình trở nên tồi tệ hơn, vì vậy chúng ta có thể dừng sớm. 

Sau khi sửa tiền tố hợp lệ, chúng tôi tính toán hậu tố dài nhất có thể không xung đột với tiền tố đó. Hậu tố được xây dựng từ cuối về phía sau và chúng tôi dừng lại ngay khi xung đột xuất hiện vì bất kỳ việc đưa thêm nào vào sẽ chỉ làm giảm tính hợp lệ. 

Kích thước được giữ là tổng độ dài tiền tố và độ dài hậu tố. Vì chúng ta đang chia mảng thành ba phần một cách hiệu quả, tiền tố, đoạn bị loại bỏ, hậu tố, nên kích thước bị loại bỏ được tính bằng phép trừ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| tôi | Tiền tố hợp lệ | Hậu tố j | Giữ tiền tố | Giữ hậu tố | Tổng số giữ lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | vâng | 0 | 0 | 3 | 3 | 
| 1 | vâng | 0 | 1 | 2 | 3 | 
| 2 | vâng | 0 | 2 | 1 | 3 | 
| 3 | vâng | 3 | 3 | 0 | 3 | 

Tất cả các giá trị đều đã khác biệt, do đó cấu hình tốt nhất sẽ giữ lại tất cả các phần tử, đưa ra câu trả lời 0. 

Điều này xác nhận rằng khi không tồn tại xung đột, việc mở rộng hậu tố luôn tiêu tốn toàn bộ mảng, không loại bỏ được. 

### Ví dụ 2 

đầu vào:```
4
1 2 1 3
```| tôi | Tiền tố hợp lệ | Hậu tố j | Giữ tiền tố | Giữ hậu tố | Tổng số giữ lại | 
| --- | --- | --- | --- | --- | --- | 
| 0 | vâng | 2 | 0 | 2 | 2 | 
| 1 | vâng | 3 | 1 | 1 | 2 | 
| 2 | không | - | - | - | - | 

Giải pháp tốt nhất giữ`[1,2]`Và`[3]`, loại bỏ đoạn giữa`[1]`, vậy đáp án là 1 

Điều này chứng tỏ cách các bản sao buộc phải xóa ở giữa và cách lựa chọn hậu tố phải tránh các giá trị xung đột với tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi tiền tố được kiểm tra một lần và hậu tố được quét từ bên phải tổng cộng tối đa n lần trong các lần lặp | 
| Không gian | O(n) | Bộ theo dõi tiền tố và hậu tố | 

Hành vi bậc hai phù hợp thoải mái trong$n \le 2000$, vì khoảng 4 triệu thao tác có thể được quản lý bằng Python dưới các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    it = inp.strip().split()
    n = int(it[0])
    a = list(map(int, it[1:]))

    ans = n
    for i in range(n + 1):
        seen = set()
        ok = True
        for k in range(i):
            if a[k] in seen:
                ok = False
                break
            seen.add(a[k])
        if not ok:
            break

        used_suffix = set()
        j = n
        while j > i:
            if a[j - 1] in seen or a[j - 1] in used_suffix:
                break
            used_suffix.add(a[j - 1])
            j -= 1

        kept = i + (n - j)
        ans = min(ans, n - kept)

    return str(ans)

# samples
assert solve_capture("3\n1 2 3\n") == "0"

# all equal
assert solve_capture("4\n5 5 5 5\n") == "3"

# single duplicate pair
assert solve_capture("4\n1 2 1 3\n") == "1"

# already distinct larger
assert solve_capture("5\n1 2 3 4 5\n") == "0"

# prefix conflict early
assert solve_capture("5\n1 2 1 2 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 3 | xóa trường hợp xấu nhất | 
| cặp trùng lặp duy nhất | 1 | loại bỏ giữa đúng | 
| đã khác biệt | 0 | trường hợp không hoạt động | 
| mẫu lặp đi lặp lại | 2 | tương tác tiền tố-hậu tố | 

## Vỏ cạnh 

Đối với một mảng như$[5, 5, 5, 5]$, thuật toán sẽ thử độ dài tiền tố. Tại$i = 1$, tiền tố là hợp lệ. Quá trình quét hậu tố ngay lập tức dừng lại sau khi thêm một phần tử vì phần tử tiếp theo xung đột với bộ hậu tố. Điều này mang lại kích thước được giữ nguyên là 2, nghĩa là kích thước loại bỏ là 2, nhưng tiền tố tốt hơn cho phép loại bỏ tối ưu là 3. Thuật toán đánh giá chính xác tất cả các lựa chọn tiền tố và tìm ra mức tối thiểu. 

Đối với trường hợp như$[1, 2, 1, 2]$, tiền tố tại$i = 2$hợp lệ, nhưng hậu tố không thể bao gồm 1 hoặc 2, do đó kích thước hậu tố trở thành 0. Kết quả tương ứng với việc loại bỏ một đoạn ở giữa có kích thước 2, đoạn này được logic phân tách ghi lại một cách chính xác.
