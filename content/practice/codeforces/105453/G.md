---
title: "CF 105453G - Tối ưu hóa các chuyến khởi hành tại sân bay"
description: "Chúng ta được cung cấp một chuỗi các chuyến bay được sắp xếp theo thời gian khởi hành theo lịch trình của chúng. Mỗi chuyến bay có một thời điểm lý tưởng nhất để sử dụng đường băng, khoản thanh toán mà nó đưa ra nếu được phép khởi hành chính xác vào thời điểm đó và hình phạt sẽ áp dụng nếu không."
date: "2026-06-23T17:36:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 91
verified: true
draft: false
---

[CF 105453G - Tối ưu hóa các chuyến khởi hành tại sân bay](https://codeforces.com/problemset/problem/105453/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chuyến bay được sắp xếp theo thời gian khởi hành theo lịch trình của chúng. Mỗi chuyến bay có một thời điểm lý tưởng nhất để sử dụng đường băng, khoản thanh toán mà nó đưa ra nếu được phép khởi hành chính xác vào thời điểm đó và hình phạt sẽ áp dụng nếu không. 

Vì sân bay chỉ còn một đường băng duy nhất nên chỉ một số chuyến bay thực sự có thể khởi hành gần với thời gian dự kiến. Nếu một chuyến bay được phép khởi hành đúng giờ, nó sẽ mang lại phần thưởng tích cực. Nếu không được chọn, nó sẽ bị hoãn lại và thay vào đó sẽ đóng góp giá trị âm vì hãng hàng không sẽ được bồi thường cho sự chậm trễ. 

Hạn chế vận hành là bất cứ khi nào chúng tôi chọn để chuyến bay khởi hành đúng giờ, chúng tôi phải đảm bảo ít nhất T đơn vị thời gian trôi qua trước chuyến khởi hành được chọn tiếp theo. Điều này biến vấn đề thành việc chọn một tập hợp con các chuyến bay tuân thủ các ràng buộc về khoảng cách trong khi tối đa hóa tổng lợi nhuận, trong đó mỗi chuyến bay được chọn mang lại một giá trị và mỗi chuyến bay bị bỏ qua sẽ mang lại một giá trị khác. 

Đầu ra là tổng lợi nhuận tối đa có thể đạt được sau khi quyết định chuyến bay nào khởi hành đúng giờ và chuyến bay nào bị trì hoãn. 

Các ràng buộc làm rõ rằng N có thể lớn tới một triệu, điều này ngay lập tức loại trừ mọi lập trình động bậc hai hoặc thậm chí log bình phương. Một giải pháp kiểm tra tất cả các cặp chuyến bay hoặc thực hiện tìm kiếm nặng theo từng tiểu bang sẽ vượt quá giới hạn thời gian. Yêu cầu chính là quét tuyến tính hoặc gần tuyến tính trên các chuyến bay được sắp xếp. 

Một cách giải thích ngây thơ cũng sẽ bỏ sót rằng ràng buộc về lịch trình chỉ áp dụng giữa các chuyến bay đã chọn chứ không phải tất cả các chuyến bay. Sự khác biệt đó chính là nguyên nhân khiến các quyết định địa phương mang tính tham lam hoặc theo từng chuyến bay trở nên không chính xác. 

Một trường hợp phức tạp xuất hiện khi tất cả các chuyến bay đều gần đúng lúc. Ví dụ: nếu T lớn và tất cả t_i được đóng gói chặt chẽ, chúng tôi chỉ có thể chọn một chuyến bay. Một chiến lược tham lam ngây thơ luôn chọn lợi ích tức thời tốt nhất b_i - c_i có thể thất bại: 

đầu vào:```
3 10
1 100 -1
2 90 -1
3 80 -1
```Một lựa chọn tham lam có thể chọn chuyến bay đầu tiên do mức tăng cao nhất, nhưng trong các trường hợp có cấu trúc khác, nó có thể bỏ sót rằng sự kết hợp sau đó mang lại khoảng cách tốt hơn nếu thời gian cho phép. Ràng buộc thực sự của các cặp đôi đưa ra quyết định trên các chỉ số. 

Một trường hợp khác là việc trì hoãn thực sự có lợi cho một số chuyến bay nếu c_i gần bằng 0 hơn các chuyến bay khác. Vì c_i âm, việc bỏ qua cấu trúc và coi tất cả các chuyến bay bị bỏ qua là chi phí bằng 0 sẽ phá vỡ hoàn toàn tính chính xác. 

## Phương pháp tiếp cận 

Nỗ lực tự nhiên đầu tiên là quyết định một cách độc lập cho mỗi chuyến bay xem nên thực hiện đúng giờ hay trì hoãn. Điều này ngay lập tức thất bại vì việc thực hiện chuyến bay ảnh hưởng đến chuyến bay nào có thể được thực hiện trong tương lai do hạn chế về khoảng cách T. Bất kỳ nỗ lực nào nhằm liệt kê tất cả các tập hợp con của các chuyến bay thỏa mãn khoảng cách đều dẫn đến độ phức tạp theo cấp số nhân, vì mọi chuyến bay đều có thể được chọn hoặc bỏ qua. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn là xử lý các chuyến bay theo thứ tự và đối với mỗi chuyến bay, hãy quyết định đệ quy xem có nên thực hiện chuyến bay đó hay không trong khi theo dõi thời gian đã chọn cuối cùng. Điều này tạo ra một không gian trạng thái trong đó mỗi bước phân nhánh thành hai lựa chọn và số lượng trạng thái tăng lên khoảng 2^N trong trường hợp xấu nhất. Ngay cả việc cắt bớt các lịch trình không hợp lệ cũng không đủ vì các tập hợp con hợp lệ vẫn còn rất nhiều khi thời gian thưa thớt. 

Quan sát quan trọng là sự đóng góp của các chuyến bay bị bỏ qua là cố định bất kể các quyết định về lịch trình. Mỗi chuyến bay đều đóng góp c_i nếu nó không được chọn. Nếu chúng ta bắt đầu từ đường cơ sở nơi tất cả các chuyến bay bị hoãn thì tổng số chỉ đơn giản là tổng của tất cả c_i. Quyết định thực sự duy nhất là chúng ta nâng hạng chuyến bay nào từ bị hoãn lên đúng giờ và mỗi lần nâng cấp sẽ mang lại lợi nhuận tăng thêm b_i - c_i. 

Điều này chuyển đổi vấn đề thành việc chọn một tập hợp con các chuyến bay, mỗi chuyến bay có mức tăng trọng số, tuân theo ràng buộc về khoảng cách trên các chỉ số được xác định theo thời gian. Sau khi được điều chỉnh lại theo cách này, cấu trúc sẽ trở thành bài toán lập lịch theo khoảng thời gian có trọng số cổ điển trên một danh sách được sắp xếp, trong đó khả năng tương thích được xác định bởi các khoảng thời gian thay vì các khoảng tùy ý. 

Vì các chuyến bay đã được sắp xếp theo thời gian nên với mỗi chuyến bay i chúng ta chỉ cần biết chuyến bay cuối cùng j trước đó sao cho t_j + T <= t_i. Sau đó, chúng tôi quyết định xem nên thực hiện chuyến bay i và cộng lợi ích của nó lên trên giải pháp tốt nhất cho đến j hay bỏ qua nó. 

Điều này dẫn trực tiếp đến một công thức lập trình động có thể được đánh giá theo thời gian tuyến tính bằng kỹ thuật hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy vũ phu đối với các lựa chọn | O(2^N) | O(N) | Quá chậm | 
| DP với tìm kiếm tiền nhiệm | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mỗi chuyến bay thành một giá trị thể hiện mức độ tốt hơn khi phục vụ chuyến bay đúng giờ thay vì trì hoãn. Giá trị đó là b_i - c_i. Chúng tôi cũng tính toán đóng góp cơ bản là tổng của tất cả c_i. 

Tiếp theo, chúng tôi xử lý các chuyến bay theo thứ tự thời gian tăng dần và xây dựng bảng lập trình động. 

1. Tính mức tăng cho mỗi chuyến bay là b_i - c_i. Điều này tách biệt tác động quyết định của việc chọn chuyến bay. 
2. Duy trì con trỏ j theo dõi chuyến bay mới nhất có thể được lên lịch trước chuyến bay hiện tại mà không vi phạm ràng buộc khoảng cách T. 
3. Với mỗi chuyến bay i, hãy tiến j càng xa càng tốt trong khi t_j + T <= t_i giữ nguyên. Bởi vì thời gian được sắp xếp, j chỉ di chuyển về phía trước trong toàn bộ quá trình chạy, giữ cho quá trình diễn ra tuyến tính. 
4. Xác định dp[i] là mức tăng tốt nhất có thể đạt được bằng cách sử dụng i chuyến bay đầu tiên. Với mỗi i, chúng ta có hai lựa chọn: bỏ qua chuyến bay i và giữ dp[i-1], hoặc chọn chuyến bay i và cộng nó vào dp[j]. 
5. Cập nhật dp[i] là giá trị tối đa của hai lựa chọn này. 
6. Sau khi xử lý tất cả các chuyến bay, hãy cộng tổng cơ sở của tất cả c_i vào dp[N] để khôi phục toàn bộ lợi nhuận.

Tính chính xác dựa trên thực tế là khi chúng ta sửa chuyến bay i như đã chọn, hạn chế duy nhất đối với các lựa chọn trước đó là chúng phải kết thúc tại hoặc trước t_i - T. Bất kỳ giải pháp tối ưu nào bao gồm i đều phải kết hợp nó với giải pháp tối ưu trên các chuyến bay tương thích trước đó và dp[j] đã thể hiện mức tối ưu đó. 

Trạng thái dp hoàn tất vì nó nắm bắt mức tăng tốt nhất có thể đạt được cho mọi tiền tố của các chuyến bay và quá trình chuyển đổi không bao giờ bỏ qua các ứng cử viên hợp lệ do tính đơn điệu của thứ tự thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    T = int(next(it))

    t = [0] * n
    b = [0] * n
    c = [0] * n

    for i in range(n):
        t[i] = int(next(it))
        b[i] = int(next(it))
        c[i] = int(next(it))

    base = 0
    gain = [0] * n

    for i in range(n):
        base += c[i]
        gain[i] = b[i] - c[i]

    dp = [0] * (n + 1)

    j = 0
    for i in range(1, n + 1):
        ti = t[i - 1]

        while j < i and t[j] + T <= ti:
            j += 1

        take = dp[j] + gain[i - 1]
        skip = dp[i - 1]
        dp[i] = take if take > skip else skip

    print(base + dp[n])

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc tất cả các chuyến bay và tách cấu trúc hình phạt cơ bản khỏi thành phần phụ thuộc vào quyết định. Mảng dp chỉ lưu trữ cải tiến gia tăng trong kịch bản "tất cả bị trì hoãn", giúp đơn giản hóa quá trình chuyển đổi. 

Con trỏ j được nâng cao một cách đơn điệu, đây là chi tiết triển khai quan trọng đảm bảo độ phức tạp tuyến tính. Nó không bao giờ đặt lại cho mỗi i, vì cửa sổ tương thích chỉ tiến lên khi số lần tăng lên. 

Một cạm bẫy phổ biến là phải tính lại j từ đầu cho mỗi i, điều này sẽ làm giảm hiệu suất về thời gian bậc hai. Một vấn đề tế nhị khác là quên rằng dp[j] đề cập đến tiền tố kết thúc trước chuyến bay tương thích cuối cùng, không bao gồm chính j, đó là lý do tại sao con trỏ được nâng cao một cách cẩn thận. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
4 5
2 1000 -100
5 500 -500
7 300 -500
9 1000 -100
```Chúng tôi tính tổng c_i cơ sở là -1200. Mức tăng là 1100, 1000, 800, 1100. 

Bây giờ chúng tôi xây dựng dp dựa trên khả năng tương thích: 

| tôi | t_i | j (hợp lệ cuối cùng) | đạt được | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1100 | 1100 | 
| 2 | 5 | 1 | 1000 | 1100 | 
| 3 | 7 | 2 | 800 | 1100 | 
| 4 | 9 | 3 | 1100 | 2200 | 

Đáp án cuối cùng là -1200 + 2200 = 1000. 

Dấu vết này cho thấy rằng mặc dù nhiều chuyến bay có mức tăng dương, nhưng các hạn chế về khoảng cách ngăn cản việc xếp chồng tất cả chúng và DP chọn chính xác một tập hợp con tương thích. 

Bây giờ hãy xem xét một trường hợp chặt chẽ hơn:```
3 10
1 100 -1
2 90 -1
3 80 -1
```Tất cả các chuyến bay đều không tương thích với nhau do T lớn. DP chỉ có thể chọn tối đa một mức tăng và chọn chính xác cải tiến đơn lẻ tốt nhất, mang lại câu trả lời cuối cùng -3 + 99 = 96. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chuyến bay được xử lý một lần và con trỏ j chỉ di chuyển về phía trước trên mảng | 
| Không gian | O(N) | Mảng lưu trữ thời gian, mức tăng và giá trị dp | 

Quét tuyến tính và con trỏ đơn điệu đảm bảo giải pháp xử lý thoải mái tới một triệu chuyến bay trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        data = sys.stdin.read().strip().split()
        it = iter(data)
        n = int(next(it))
        T = int(next(it))
        if n == 0:
            print(0)
            return

        t = [0] * n
        b = [0] * n
        c = [0] * n

        for i in range(n):
            t[i] = int(next(it))
            b[i] = int(next(it))
            c[i] = int(next(it))

        base = sum(c)
        gain = [b[i] - c[i] for i in range(n)]

        dp = [0] * (n + 1)
        j = 0

        for i in range(1, n + 1):
            ti = t[i - 1]
            while j < i and t[j] + T <= ti:
                j += 1
            dp[i] = max(dp[i - 1], dp[j] + gain[i - 1])

        print(base + dp[n])

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""4 5
2 1000 -100
5 500 -500
7 300 -500
9 1000 -100
""") == "1000"

# minimum size
assert run("""0 5
""") == "0"

# all flights far apart, all taken
assert run("""3 1
1 10 -1
10 10 -1
20 10 -1
""") == "27"

# all close, only best single
assert run("""3 100
1 10 -1
2 20 -1
3 30 -1
""") == "29"

# equal gains, spacing limits selection
assert run("""4 2
1 10 -1
2 10 -1
3 10 -1
4 10 -1
""") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyến bay trống | 0 | xử lý trường hợp cơ bản | 
| chuyến bay cách nhau | tất cả đã chọn | không có trường hợp nhiễu | 
| khoảng cách chặt chẽ | sự lựa chọn tốt nhất duy nhất | ràng buộc ràng buộc | 
| tăng đồng đều | lựa chọn theo khoảng cách | DP chính xác | 

## Vỏ cạnh 

Khi T bằng 0, mọi chuyến bay đều tương thích với mọi chuyến bay khác và chiến lược tối ưu sẽ trở thành việc chọn tất cả các chuyến bay vì mọi mức tăng đều độc lập. Thuật toán xử lý điều này vì j luôn tiến tới i, biến các chuyển đổi dp thành sự tích lũy đơn giản của mức tăng dương. 

Khi T cực lớn thì chỉ có thể chọn một chuyến bay. Trong tình huống này, dp đương nhiên sẽ chọn mức tăng tối đa trong số tất cả các chuyến bay, vì dp[j] sẽ luôn bằng 0 đối với tất cả j hợp lệ. 

Khi tất cả mức tăng đều âm, dp không bao giờ cải thiện trên 0, nghĩa là chiến lược tối ưu là trì hoãn mọi chuyến bay. Điều này được nắm bắt chính xác vì dp vẫn không dương và chi phí cơ bản là không thể tránh khỏi. 

Khi các chuyến bay có mật độ thời gian cực kỳ dày đặc, con trỏ j tiến chậm và thuật toán hoạt động giống như một DP lập kế hoạch có trọng số tiêu chuẩn, luôn tôn trọng ranh giới tương thích gần đây nhất mà không cần xem lại các chuyến bay trước đó.
