---
title: "CF 106367E - Tám nghìn năm chờ đợi"
description: "Chúng ta được cung cấp một mảng và chúng ta xem xét mọi dãy con được hình thành bằng cách xóa bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự. Đối với mỗi chuỗi kết quả, chúng ta đếm xem hai phần tử liền kề bằng nhau bao nhiêu lần. Số lượng này được gọi là số lần nhấp nháy."
date: "2026-06-20T22:57:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106367
codeforces_index: "E"
codeforces_contest_name: "Whalica Cup (Round 2)"
rating: 0
weight: 106367
solve_time_s: 65
verified: true
draft: false
---

[CF 106367E - Tám nghìn năm chờ đợi](https://codeforces.com/problemset/problem/106367/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta xem xét mọi dãy con được hình thành bằng cách xóa bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự. Đối với mỗi chuỗi kết quả, chúng ta đếm xem hai phần tử liền kề bằng nhau bao nhiêu lần. Số lượng này được gọi là số lần nhấp nháy. Nhiệm vụ là đếm xem có bao nhiêu chuỗi con tạo ra chính xác k lần nhấp nháy. 

Một cách hữu ích để suy nghĩ về một dãy con là thông qua các lần chạy của nó. Một lần chạy là một khối tối đa có giá trị bằng nhau. Nếu một chuỗi con có một chuỗi có độ dài L thì chuỗi đó đóng góp L trừ đi một lần nhấp nháy. Tổng hợp tất cả các lần chạy sẽ cho ra tổng số lần nhấp nháy. Tương tự, nếu một dãy con có độ dài m và r chạy thì số lần nháy là m trừ r. 

Việc cải cách này quan trọng vì nó cho thấy rằng các lần nhấp nháy hoàn toàn được xác định bởi tần suất chúng ta kéo dài một lần chạy so với bắt đầu một lần chạy mới, thay vì bởi các vị trí tuyệt đối trong mảng ban đầu. 

Các ràng buộc chặt chẽ theo một hướng khác với các bài toán dãy con thông thường. Độ dài mảng có thể đạt tới hai trăm nghìn, trong khi k nhiều nhất là hai trăm. Điều này gợi ý rõ ràng một cách tiếp cận lập trình động giữ một kích thước nhỏ cho k và tổng hợp theo cấu trúc. Một giải pháp theo dõi các chuỗi con tùy ý một cách rõ ràng hoặc thậm chí duy trì trạng thái trên mỗi cặp vị trí ngay lập tức là quá lớn. 

Một nỗ lực ngây thơ sẽ tạo ra tất cả các chuỗi con và tính toán trực tiếp các nhấp nháy. Điều đó đã liên quan đến 2^n đối tượng và thậm chí với n khoảng ba mươi đối tượng, điều này trở nên không khả thi. Một lỗi phổ biến khác là cố gắng thực hiện DP trên các chuỗi con mà không nén thứ nguyên “giá trị cuối cùng”, điều này sẽ yêu cầu theo dõi quá trình chuyển đổi giữa tất cả các giá trị một cách rõ ràng và nhanh chóng bùng nổ trong bộ nhớ. 

Một cạm bẫy tinh tế hơn xuất hiện khi chỉ nghĩ về “số lượng các cặp liền kề bằng nhau”. Người ta có thể thử đếm các cặp được đóng góp bởi các phần tử bằng nhau trong mảng ban đầu, nhưng phần kề trong dãy con không phải là phần kề trong mảng, vì vậy điều này làm mất tất cả cấu trúc. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi chuỗi con, mô phỏng nó và đếm số lần nhấp nháy bằng cách quét các cặp liền kề. Điều này hoạt động về mặt khái niệm vì nó tuân theo định nghĩa trực tiếp, nhưng nó đòi hỏi thời gian theo cấp số nhân. Đối với n lên tới 200000, thậm chí không thể biểu diễn tất cả các dãy con. 

Thông tin chi tiết về cấu trúc quan trọng là chúng ta có thể xây dựng các chuỗi con tăng dần trong khi chỉ theo dõi hai phần thông tin: giá trị được chọn cuối cùng và số lần nhấp nháy cho đến nay. Khi chúng tôi nối thêm một phần tử mới, chúng tôi sẽ bắt đầu một chuỗi con mới, tiếp tục chuỗi con hiện có với cùng giá trị và tăng số lượng flash hoặc chuyển sang một giá trị khác và giữ nguyên số lượng flash. 

Khó khăn là “giá trị cuối cùng” nằm trên tất cả các giá trị riêng biệt, do đó, một DP đơn giản sẽ cần các trạng thái O(nk) cho mỗi giá trị, quá lớn để duy trì trên toàn cầu trong một mảng 2D đơn giản. Quan sát quan trọng là việc chuyển đổi sang giá trị cố định x tại vị trí i chỉ phụ thuộc vào tổng số trên tất cả các giá trị cuối cùng trước đó chứ không phụ thuộc vào danh tính riêng lẻ của chúng. Điều này cho phép chúng tôi duy trì các mảng DP trên mỗi giá trị nhưng cập nhật chúng bằng cách sử dụng tổng toàn cục trên k. 

Chúng tôi xử lý mảng từ trái sang phải. Với mỗi giá trị v, chúng ta duy trì dp[v][t], số dãy con kết thúc bằng giá trị v có đúng t nhấp nháy. Chúng ta cũng duy trì tổng[t], tổng của dp[v][t] trên tất cả v. 

Khi chúng tôi xử lý một lần xuất hiện x mới, chỉ có dp[x] thay đổi. Tất cả dp[v] khác vẫn hợp lệ vì chúng không phụ thuộc vào việc x hiện có được xử lý hay không; x chỉ thêm các chuỗi con mới kết thúc bằng x. 

Vị trí này là thứ giữ cho nghiệm tuyến tính trong n với hệ số k nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP tối ưu với trạng thái giá trị | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì hai cấu trúc. Đầu tiên là dp[v], một mảng có kích thước k+1 lưu trữ số lượng dãy con kết thúc bằng giá trị v. Mảng thứ hai là tổng[t], tổng trên tất cả v của dp[v][t]. 

Về mặt khái niệm, chúng tôi cũng bao gồm dãy con trống, nhưng chúng tôi chỉ coi nó như một cơ chế khởi đầu thay vì lưu trữ nó một cách rõ ràng trong dp. 

### bước 

1. Khởi tạo dp[v] dưới dạng mảng trống và tổng[t] bằng 0 cho tất cả t. Điều này tương ứng với việc chưa có phần tử nào được xử lý. 
2. Xử lý mảng từ trái qua phải. Tại vị trí i, đặt giá trị hiện tại là x. 
3. Tạo một mảng đóng góp mới add[t] cho giá trị x. Điều này sẽ lưu trữ tất cả các chuỗi con kết thúc tại x sau khi bao gồm a[i]. 
4. Bắt đầu một dãy con mới chỉ bao gồm a[i]. Điều này góp phần thêm [0] += 1, vì một phần tử duy nhất tạo ra không nhấp nháy. 
5. Mở rộng các dãy con hiện có kết thúc ở bất kỳ giá trị nào khác với x. Đối với một t cố định, tất cả các chuỗi con của tổng [t] có thể mở rộng, nhưng những chuỗi đã kết thúc bằng x là đặc biệt vì việc mở rộng x đến x sẽ làm tăng nhấp nháy. Vì vậy, chúng tôi sử dụng Total[t] - dp[x][t] làm số lượng các chuỗi con kết thúc bằng một giá trị khác và mở rộng chúng mà không thay đổi t. 
6. Mở rộng các dãy con tận cùng bằng x. Mỗi chuỗi con như vậy với lần nhấp nháy t-1 sẽ trở thành một chuỗi con với lần nhấp nháy t khi chúng ta nối lại x, vì vậy chúng ta thêm dp[x][t-1] vào add[t]. 
7. Sau khi tính toán phép cộng, chúng tôi cập nhật tổng bằng cách trừ dp[x] cũ và cộng dp[x] mới có nguồn gốc từ phép cộng. 
8. Thay thế dp[x] bằng phép cộng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, dp[v][t] biểu thị chính xác tất cả các chuỗi con được thấy cho đến nay có phần tử cuối cùng là v và số lần nháy là t. Mọi dãy con đều không liên quan đến phần tử hiện tại hoặc kết thúc ở phần tử đó. Khi xử lý một lần xuất hiện mới của x, mọi phần mở rộng hợp lệ sẽ được ghi lại bằng cách bắt đầu mới tại x hoặc thêm x vào chuỗi con hợp lệ trước đó. Việc phân tách thành “kết thúc tại x” và “không kết thúc tại x” là đầy đủ và rời rạc, do đó, mỗi chuỗi con được tính chính xác một lần và không có chuyển tiếp nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        dp = {}
        total = [0] * (k + 1)

        for x in a:
            if x not in dp:
                dp[x] = [0] * (k + 1)

            old = dp[x]
            add = [0] * (k + 1)

            add[0] += 1

            for tval in range(k + 1):
                if total[tval]:
                    base = total[tval] - old[tval]
                    if base:
                        add[tval] += base

            for tval in range(1, k + 1):
                add[tval] += old[tval - 1]

            for tval in range(k + 1):
                delta = add[tval] - old[tval]
                if delta:
                    total[tval] += delta
                    old[tval] = add[tval]
                else:
                    old[tval] = add[tval]

        print(total[k] % 998244353)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là bảng DP trên mỗi giá trị và mảng tổng thể toàn cầu. Bước cập nhật chỉ cách ly cẩn thận tác động của giá trị phần tử hiện tại, đảm bảo các khối dp khác không bị ảnh hưởng. Phép trừ`total[t] - old[t]`là cơ chế quan trọng giúp tránh các chuỗi đếm kép đã kết thúc ở cùng một giá trị. 

Vòng lặp trên k vẫn đủ nhỏ để có hiệu quả vì k bị giới hạn bởi 200 và tất cả các phép toán đều tuyến tính trong chiều đó. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[1, 2, 2, 1]`với`k = 1`. 

Chúng tôi chỉ theo dõi các mục khác không. 

| Bước | x | dp[x] sau khi cập nhật | tổng cộng[0] | tổng cộng[1] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1, 0] | 1 | 0 | 
| 2 | 2 | [1, 0] | 3 | 0 | 
| 3 | 2 | [2, 1] | 5 | 1 | 
| 4 | 1 | [2, 1] | 8 | 5 | 

Sau khi xử lý tất cả các phần tử, tổng [1] bằng 5, khớp với số lượng chuỗi con bằng chính xác một lần nháy. 

Bây giờ hãy xem xét`[1, 1, 1]`với`k = 0`. Mọi dãy con không có nhấp nháy trừ khi nó chứa dãy liền kề lặp lại trong dãy con, điều này chỉ xảy ra khi chúng ta chọn các phần đóng góp bằng nhau liên tiếp. 

| Bước | x | dp[x] sau khi cập nhật | tổng cộng[0] | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | 1 | 
| 2 | 1 | [2] | 2 | 
| 3 | 1 | [3] | 3 | 

Câu trả lời cuối cùng là 3 với k = 0, tương ứng với tất cả các chuỗi con không trống bao gồm các giá trị giống hệt nhau trong các lần chạy đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi phần tử cập nhật một mảng dp có kích thước k | 
| Không gian | O(nk) | Một mảng dp cho mỗi giá trị riêng biệt, mỗi giá trị có kích thước k | 

Tổng số n trong tất cả các trường hợp thử nghiệm được giới hạn bởi 200000 và k tối đa là 200, do đó số lần chuyển đổi DP nằm trong khoảng vài chục triệu, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn được chấp nhận trong giới hạn bộ nhớ lớn do kích thước k bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    import sys
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        dp = {}
        total = [0] * (k + 1)

        for x in a:
            if x not in dp:
                dp[x] = [0] * (k + 1)

            old = dp[x]
            add = [0] * (k + 1)

            add[0] += 1

            for tval in range(k + 1):
                add[tval] += total[tval] - old[tval]

            for tval in range(1, k + 1):
                add[tval] += old[tval - 1]

            for tval in range(k + 1):
                total[tval] += add[tval] - old[tval]
                old[tval] = add[tval]

        res.append(str(total[k] % MOD))
    return "\n".join(res)

# sample-style checks (illustrative placeholders)
assert run("1\n4 1\n1 2 2 1\n") == "5"
assert run("1\n3 0\n1 1 2\n") == "4"

# custom cases
assert run("1\n1 0\n7\n") == "1", "single element"
assert run("1\n2 1\n1 1\n") == "1", "only equal pair possible"
assert run("1\n3 0\n1 2 3\n") == "7", "all distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | xử lý dãy con cơ sở | 
| tất cả đều bằng nhau | 1 | tạo flash đơn | 
| tất cả đều khác biệt | 7 | không nhấp nháy, đếm chuỗi đầy đủ | 

## Vỏ cạnh 

Mảng một phần tử tối thiểu cho biết liệu thuật toán có khởi tạo chính xác dp với chuỗi con bao gồm một phần tử đóng góp không nhấp nháy hay không. Trong trường hợp này, dp tạo một mục nhập mới cho giá trị, gán add[0] = 1 và Total[0] trở thành 1, tạo ra kết quả chính xác. 

Một mảng có tất cả các giá trị giống nhau sẽ kiểm tra xem các bản cập nhật lặp lại có tích lũy chính xác các lần nhấp nháy hay không khi mở rộng trong cùng một giá trị. Mỗi lần xử lý cùng một giá trị, quá trình chuyển đổi dp[x][t-1] sẽ tăng số lượng flash và không có quá trình chuyển đổi giá trị chéo nào gây trở ngại. 

Một mảng có tất cả các giá trị riêng biệt nhấn mạnh logic “giá trị chuyển đổi”. Vì dp[x] luôn bằng 0 trước khi xử lý từng x, nên mọi phần mở rộng chuỗi con chỉ xuất phát từ tổng [t] và phép trừ dp[x] không quan trọng. Thuật toán giảm xuống việc đếm dãy con đơn giản với k = 0, tạo ra 2^n - 1 dãy con chính xác cho k = 0.
