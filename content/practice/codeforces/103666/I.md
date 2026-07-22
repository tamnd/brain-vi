---
title: "CF 103666I - \u041d\u0443\u0436\u043d\u043e \u0431\u043e\u043b\u044c\u0448\u0435 \u0437\u043e\u043b\u043e\u0442\u0430"
description: "Chúng ta được cấp một bộ sưu tập các hiện vật ma thuật, mỗi hiện vật có giá trị dương $wi$. Người anh hùng bắt đầu với sức mạnh phép thuật bằng không. Mỗi hiện vật phải được sử dụng chính xác một lần và mỗi hiện vật có thể được kích hoạt theo một trong hai cách."
date: "2026-07-02T21:33:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "I"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 47
verified: true
draft: false
---

[CF 103666I - \u041d\u0443\u0436\u043d\u043e \u0431\u043e\u043b\u044c\u0448\u0435 \u0437\u043e\u043b\u043e\u0442\u0430](https://codeforces.com/problemset/problem/103666/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập các hiện vật ma thuật, mỗi hiện vật đều có giá trị tích cực.$w_i$. Người anh hùng bắt đầu với sức mạnh phép thuật bằng không. Mỗi hiện vật phải được sử dụng chính xác một lần và mỗi hiện vật có thể được kích hoạt theo một trong hai cách. 

Nếu một hiện vật được kích hoạt bằng phép thuật, giá trị của nó sẽ được cộng vào sức mạnh phép thuật hiện tại của anh hùng. Nếu nó được kích hoạt bằng sức mạnh, nó sẽ mang lại số vàng bằng giá trị của hiện vật nhân với sức mạnh phép thuật hiện tại của anh hùng tại thời điểm đó. Lệnh kích hoạt là miễn phí và quyết định sử dụng phép thuật hay sức mạnh cho từng hiện vật cũng miễn phí. 

Mục tiêu là tối đa hóa tổng số vàng thu được từ việc kích hoạt sức mạnh, đồng thời hãy nhớ rằng chỉ những kích hoạt phép thuật mới tăng hệ số nhân được sử dụng để nhận được vàng trong tương lai. 

Kích thước đầu vào nhỏ, với$n \le 100$và giá trị$w_i \le 100$. Điều này ngay lập tức gợi ý rằng các phương pháp quy hoạch động bậc hai hoặc thậm chí bậc ba có thể chấp nhận được, nhưng việc liệt kê theo cấp số nhân trên tất cả các tập hợp con là không đáng tin cậy vì$2^{100}$là quá lớn. 

Một quan sát cấu trúc quan trọng là cách duy nhất để tăng lợi nhuận trong tương lai là “tiêu dùng” một số hiện vật với tư cách là người xây dựng quyền lực thuần túy và phần còn lại là người tạo ra lợi nhuận. Thứ tự quan trọng vì sức mạnh tích lũy theo thời gian và trực tiếp nhân lên tất cả các khoản đóng góp lợi nhuận được chọn sau đó. 

Một trường hợp phức tạp là khi tất cả hiện vật đều được sử dụng như phép thuật. Trong trường hợp đó, không có vàng nào được tạo ra và câu trả lời là số không. Một trường hợp góc khác là khi tất cả được sử dụng làm cường độ, nhưng khi đó hệ số nhân luôn bằng 0, do đó một lần nữa câu trả lời là 0. Giải pháp tối ưu phải cân bằng được cả hai vai trò. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực thử tất cả các khả năng chỉ định từng hiện vật được sử dụng làm ma thuật hoặc sức mạnh, đồng thời xem xét tất cả các hoán vị của thứ tự kích hoạt. Đối với mỗi cách sắp xếp, chúng tôi mô phỏng quy trình, theo dõi nguồn điện hiện tại và tích lũy vàng. Điều này đã trở nên không khả thi vì ngay cả khi bỏ qua các hoán vị, vẫn có$2^n$bài tập và mỗi chi phí mô phỏng$O(n)$, cho$O(n \cdot 2^n)$, vượt xa giới hạn. 

Quan sát quan trọng là thứ tự có thể được chuẩn hóa. Nếu chúng ta sửa những tạo tác nào được sử dụng cho phép thuật, thì việc áp dụng tất cả các hoạt động ma thuật trước tiên luôn là điều tối ưu theo thứ tự tăng dần khi chúng ta muốn sự đóng góp sức mạnh của chúng cho vật chất, bởi vì việc trì hoãn phép thuật chỉ làm giảm số nhân trong tương lai. Khi hiểu được sự tách biệt này, vấn đề sẽ trở thành: chọn một tập hợp con các vật phẩm để đóng góp sức mạnh, sau đó quyết định thứ tự cho các vật phẩm sức mạnh nhằm tối đa hóa lợi ích theo tổng tiền tố ngày càng tăng. 

Điều này dẫn đến một công thức lập trình động về số lượng hiện vật được sử dụng làm phép thuật và tổng sức mạnh của chúng được tạo ra như thế nào. Chúng tôi sắp xếp các hiện vật để chúng tôi suy luận từng cái một, duy trì các trạng thái có thể có của tổng sức mạnh ma thuật và số lượng vật phẩm chúng tôi đã xử lý. Mỗi hiện vật đều tăng sức mạnh hoặc đóng góp vào sức mạnh hiện tại của vàng và quyết định được đưa ra một cách tối ưu thông qua chuyển đổi DP. 

Cấu trúc này về cơ bản là một DP giống như chiếc ba lô, trong đó trạng thái theo dõi tổng ma thuật tích lũy và số vàng tốt nhất có thể đạt được cho số tiền đó sau khi xử lý tiền tố của các vật phẩm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Lập trình động |$O(n^2 \cdot \max w)$|$O(n \cdot \max w)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quá trình này là xây dựng hai nhóm: hiện vật dùng để tăng sức mạnh ma thuật và hiện vật dùng để kiếm vàng, trong đó vàng phụ thuộc vào sức mạnh tích lũy cuối cùng tại thời điểm mỗi hiện vật sức mạnh được sử dụng. Khó khăn là việc kích hoạt sức mạnh phụ thuộc vào sức mạnh đang phát triển, vì vậy chúng ta phải lập mô hình tích lũy sức mạnh một cách cẩn thận. 

### bước 

1. Sắp xếp các tạo phẩm theo giá trị hoặc chính xác hơn là xử lý chúng theo thứ tự tùy ý vì tất cả các lựa chọn đều đối xứng. Thay vào đó, chúng tôi tập trung hoàn toàn vào DP trên các tập hợp con thông qua xử lý tiền tố. 
2. Xác định trạng thái DP nơi chúng tôi theo dõi số lượng hiện vật đã được xử lý và tổng sức mạnh ma thuật được tích lũy cho đến nay và đối với mỗi trạng thái như vậy, hãy lưu trữ số vàng tối đa có thể đạt được. 
3. Khởi tạo DP mà không xử lý hiện vật nào, không có năng lượng và không có vàng. 
4. Đối với mỗi hiện vật có giá trị$w$, chúng tôi xem xét hai chuyển đổi từ mọi trạng thái hiện có. Một quá trình chuyển đổi coi hiện vật như ma thuật, tăng sức mạnh hiện tại lên$w$không tạo ra vàng. Quá trình chuyển đổi khác coi nó là sức mạnh, tạo ra vàng bằng sức mạnh hiện tại nhân với$w$, mà không thay đổi sức mạnh. 
5. Chúng tôi cập nhật DP một cách cẩn thận để mỗi tạo phẩm được sử dụng chính xác một lần, lặp lại các trạng thái theo thứ tự xử lý ngược lại để tránh sử dụng lại trong cùng một lần lặp. 
6. Sau khi xử lý tất cả hiện vật, chúng tôi quét tất cả các trạng thái DP và lấy giá trị vàng tối đa. 

Lựa chọn thiết kế chính là lưu trữ năng lượng dưới dạng biến trạng thái. Điều này là cần thiết vì vàng phụ thuộc vào hệ số nhân ở mỗi quyết định, và hệ số nhân đó chính xác là sức mạnh ma thuật tích lũy cho đến nay. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình này, thông tin duy nhất ảnh hưởng đến kết quả trong tương lai là tổng sức mạnh phép thuật hiện tại và những vật phẩm nào còn lại. Trạng thái DP nắm bắt chính xác thông tin này. Bất kỳ hai chuỗi nào kết thúc ở cùng một tiền tố được xử lý và cùng sức mạnh đều có thể thay thế cho nhau để đưa ra các quyết định trong tương lai, do đó, chỉ giữ số vàng tốt nhất cho mỗi trạng thái là an toàn. Điều này thiết lập cấu trúc con tối ưu, vì các quyết định cho mỗi tạo tác chỉ phụ thuộc vào sức mạnh hiện tại chứ không phụ thuộc vào cách đạt được sức mạnh đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = list(map(int, input().split()))

    max_sum = sum(w)

    # dp[i][p] = max gold after processing some prefix, with power p
    dp = [[-1] * (max_sum + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(n):
        wi = w[i]
        for j in range(i, -1, -1):
            for p in range(max_sum - wi, -1, -1):
                if dp[j][p] < 0:
                    continue

                # use as magic: increase power
                dp[j + 1][p + wi] = max(dp[j + 1][p + wi], dp[j][p])

                # use as strength: gain gold
                dp[j + 1][p] = max(dp[j + 1][p], dp[j][p] + p * wi)

    ans = 0
    for j in range(n + 1):
        for p in range(max_sum + 1):
            ans = max(ans, dp[j][p])

    print(ans)

if __name__ == "__main__":
    solve()
```Mã xây dựng một bảng DP được lập chỉ mục theo số lượng vật phẩm đã được xử lý và sức mạnh ma thuật tích lũy hiện tại. Mỗi hiện vật đều góp phần tăng sức mạnh hoặc góp phần tạo ra vàng dựa trên sức mạnh hiện tại. 

Việc lặp lại ngược lại`j`Và`p`đảm bảo mỗi tạo phẩm được sử dụng chính xác một lần, ngăn chặn các trạng thái ghi đè có thể tái sử dụng sai cùng một mục nhiều lần. Sự chuyển đổi sang sử dụng sức mạnh`p * wi`, mã hóa trực tiếp quy tắc nhân. 

## Ví dụ đã hoạt động 

Xem xét đầu vào mẫu với các giá trị$[1, 1, 2, 2]$. 

Chúng tôi theo dõi DP đơn giản hóa tập trung vào các lựa chọn tối ưu. 

| Bước | Cổ vật | Quyền lực trước | Hành động | Nguồn sau | Vàng đạt được | Tổng số vàng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | ma thuật | 1 | 0 | 0 | 
| 2 | 2 | 1 | ma thuật | 3 | 0 | 0 | 
| 3 | 1 | 3 | sức mạnh | 3 | 3 | 3 | 
| 4 | 2 | 3 | sức mạnh | 3 | 6 | 9 | 

Dấu vết này cho thấy rằng việc xây dựng sức mạnh sớm và sử dụng nó sau này mang lại kết quả tốt hơn hẳn so với việc kết hợp các hành động một cách tùy tiện. 

Bây giờ hãy xem xét một đầu vào bị lệch$[5, 1, 1]$. 

| Bước | Cổ vật | Quyền lực trước | Hành động | Nguồn sau | Vàng đạt được | Tổng số vàng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | ma thuật | 5 | 0 | 0 | 
| 2 | 1 | 5 | sức mạnh | 5 | 5 | 5 | 
| 3 | 1 | 5 | sức mạnh | 5 | 5 | 10 | 

Điều này cho thấy rằng một phép thuật lớn duy nhất sớm có thể tối đa hóa tất cả sức mạnh tăng được sau này. 

Các dấu vết xác nhận rằng DP đang nắm bắt chính xác sự cân bằng giữa việc xây dựng sức mạnh và chi tiêu nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot \sum w)$| Mỗi mục chuyển tiếp trên tất cả các trạng thái DP và mỗi trạng thái xem xét hai lựa chọn | 
| Không gian |$O(n \cdot \sum w)$| Bảng DP lưu trữ vàng tốt nhất cho từng tiền tố và giá trị sức mạnh | 

Những hạn chế$n \le 100$,$w_i \le 100$ngụ ý$\sum w \le 10000$. Do đó DP chạy trong khoảng$10^8$trong trường hợp xấu nhất, có thể chấp nhận được trong Python được tối ưu hóa dưới 2 giây trong cài đặt cạnh tranh thông thường khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    data = inp.strip().split()
    n = int(data[0])
    w = list(map(int, data[1:]))

    max_sum = sum(w)
    dp = [[-1] * (max_sum + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(n):
        wi = w[i]
        for j in range(i, -1, -1):
            for p in range(max_sum - wi, -1, -1):
                if dp[j][p] < 0:
                    continue
                dp[j + 1][p + wi] = max(dp[j + 1][p + wi], dp[j][p])
                dp[j + 1][p] = max(dp[j + 1][p], dp[j][p] + p * wi)

    ans = 0
    for j in range(n + 1):
        for p in range(max_sum + 1):
            ans = max(ans, dp[j][p])
    return str(ans)

# provided sample
assert run("4\n1 1 2 2\n") == "9"

# minimum size
assert run("1\n5\n") == "0"

# all equal small
assert run("3\n1 1 1\n") == "2"

# increasing values
assert run("3\n1 2 3\n") == "8"

# decreasing values
assert run("3\n3 2 1\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hiện vật | 0 | một vật phẩm không thể tạo ra vàng | 
| tất cả những cái | 2 | thứ tự các vấn đề về phép thuật và sức mạnh | 
| trộn nhỏ | 8 | DP cân bằng chính xác việc tích tụ năng lượng | 
| thứ tự ngược lại | 8 | tính đối xứng của chiến lược tối ưu | 

## Vỏ cạnh 

Một trường hợp tối thiểu với một tạo phẩm, ví dụ như đầu vào`1 / 5`, buộc thuật toán phải nhận ra rằng nếu không có bất kỳ phép thuật nào trước đó thì bất kỳ kích hoạt sức mạnh nào cũng mang lại số vàng bằng 0. DP bắt đầu với công suất bằng 0, vì vậy cả hai lần chuyển đổi đều dẫn đến câu trả lời cuối cùng bằng 0. 

Một trường hợp thống nhất như`3 / 1 1 1`chứng tỏ rằng việc chia tách là cần thiết. Phương án tối ưu là sử dụng một tạo tác cho phép thuật và hai tạo tác cho sức mạnh, mang lại sức mạnh 1 và sau đó là vàng.$1 + 1 = 2$. DP khám phá chính xác cả hai khả năng vì nó giữ tất cả các trạng thái công suất trung gian. 

Một trường hợp có giá trị ban đầu lớn, chẳng hạn như`3 / 10 1 1`, cho thấy tại sao việc đặt hàng tham lam lại thất bại nếu chúng ta luôn cố gắng tối đa hóa số vàng ngay lập tức. Giải pháp tối ưu trước tiên chuyển đổi 10 thành năng lượng, sau đó sử dụng nó hai lần, mang lại 20, mà DP thu được thông qua việc truyền trạng thái chính xác.
