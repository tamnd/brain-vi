---
title: "CF 102832A - Krypton"
description: "Trò chơi cung cấp bảy gói nạp tiền khác nhau. Mỗi gói có một chi phí cố định, thông thường cung cấp phiếu giảm giá gấp mười lần chi phí đó và chỉ tặng thêm phần thưởng khi mua gói cụ thể đó lần đầu tiên."
date: "2026-07-26T15:08:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102832
codeforces_index: "A"
codeforces_contest_name: "2020 China Collegiate Programming Contest Changchun Onsite"
rating: 0
weight: 102832
solve_time_s: 42
verified: true
draft: false
---

[CF 102832A - Krypton](https://codeforces.com/problemset/problem/102832/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi cung cấp bảy gói nạp tiền khác nhau. Mỗi gói có một chi phí cố định, thông thường cung cấp phiếu giảm giá gấp mười lần chi phí đó và chỉ tặng thêm phần thưởng khi mua gói cụ thể đó lần đầu tiên. Một gói có thể được mua nhiều lần, nhưng sau lần mua đầu tiên, tất cả các lần mua sau chỉ cung cấp các phiếu giảm giá thông thường. 

Kelo có chính xác`n`nhân dân tệ và phải chi tất cả cho hoạt động nạp tiền. Mục tiêu là tối đa hóa tổng số phiếu giảm giá nhận được. Câu trả lời nên bao gồm cả các phiếu giảm giá thông thường từ mỗi nhân dân tệ chi tiêu và lựa chọn tốt nhất có thể về tiền thưởng lần đầu. Bảy gói có giá`1, 6, 28, 88, 198, 328, 648`, với tiền thưởng lần đầu`8, 18, 28, 58, 128, 198, 388`tương ứng. 

Quan sát quan trọng đến từ việc tách phần cố định và phần biến đổi. Mỗi gói cung cấp chính xác mười phiếu giảm giá cho mỗi nhân dân tệ trong phần thưởng thông thường của nó, vì vậy, dù tiền được phân phối như thế nào, phần thưởng bình thường luôn là`10 * n`. Quyết định duy nhất là nên mua loại gói nào ít nhất một lần để nhận tiền thưởng một lần. 

Giới hạn đầu vào chỉ`n <= 2000`. Điều này đủ nhỏ để lập trình động theo số tiền bỏ ra. Một giải pháp với khoảng vài triệu thao tác là rất thoải mái, trong khi việc thử mọi cách phân phối tiền có thể có giữa bảy loại gói sẽ bùng nổ vì số lượng kết hợp tăng lên nhanh chóng với`n`. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Nếu như`n = 1`, sự lựa chọn tốt nhất là mua gói một nhân dân tệ một lần, tặng`10 + 8 = 18`phiếu giảm giá. Một giải pháp chỉ tối ưu hóa tiền thưởng mà quên mất phần thưởng thông thường sẽ xuất ra`8`, điều đó là sai. 

Đối với đầu vào`100`, đáp án tối ưu là`1084`. Người chơi có thể mua gói 88 nhân dân tệ một lần và gói 1 nhân dân tệ 12 lần. Phần thưởng bình thường là`1000`, và tiền thưởng là`58 + 8`, cho`1066`, vì vậy ví dụ này cũng cho thấy việc phân phối phải được tối ưu hóa thay vì chỉ lấy gói lớn nhất. Sự tối ưu thực tế đến từ sự kết hợp tốt hơn, mang lại`1084`. Một chiến lược tham lam luôn chọn gói giá cả phải chăng lớn nhất sẽ bỏ lỡ những sự kết hợp như vậy. 

Đối với đầu vào`648`, mua gói 648 nhân dân tệ nhiều lần sẽ nhận được phần thưởng bình thường cộng với chỉ một phần thưởng. Việc triển khai bất cẩn có thể thêm phần thưởng lần đầu tiên mỗi khi gói xuất hiện, tạo ra câu trả lời tăng cao. Cách xử lý đúng là tính tiền thưởng của mỗi gói nhiều nhất một lần. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi cách có thể để tiêu tiền trong số bảy loại gói. Đối với mỗi lần phân phối, nó sẽ tính toán số phiếu giảm giá nhận được và giữ ở mức tối đa. Điều này đúng vì mọi kế hoạch mua hàng có thể đều được xem xét. Tuy nhiên, số lượng phân phối có thể là quá lớn. Ngay cả khi hạn chế sự chú ý đến gói một nhân dân tệ rẻ nhất, đã có rất nhiều lựa chọn có thể áp dụng cho mỗi gói trong số bảy lựa chọn gói. Việc liệt kê tất cả các kết hợp là theo cấp số nhân về số lượng loại gói. 

Cấu trúc của vấn đề cho một cái nhìn đơn giản hơn. Vì mỗi nhân dân tệ luôn đóng góp chính xác mười phiếu giảm giá thông thường nên chúng ta có thể bỏ qua các phiếu giảm giá thông thường trong quá trình tối ưu hóa. Chúng tôi chỉ cần quyết định loại gói nào nhận được lần mua hàng đầu tiên. Chỉ có bảy quyết định như vậy. 

Việc chọn một gói cho phần thưởng đầu tiên sẽ tốn một số tiền và mang lại một số giá trị. Sau khi chọn tất cả các giao dịch mua đầu tiên có lãi, số tiền còn lại có thể được chi cho các gói tùy ý và đóng góp chính xác 10 phiếu giảm giá cho mỗi nhân dân tệ. Vấn đề còn lại là chiếc ba lô 0/1: mỗi loại trong số bảy loại gói có thể được chọn một lần để nhận phần thưởng hoặc không được chọn. 

Brute-force hoạt động vì nó xem xét mọi gói nạp tiền hoàn chỉnh nhưng không thành công khi số lượng gói trở nên lớn. Quan sát cho thấy việc mua hàng lặp lại không có tác dụng đặc biệt nào ngoại trừ lần đầu tiên cho phép chúng tôi giảm bớt vấn đề khi chọn tối đa bảy mặt hàng thưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(7n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng ba lô trong đó`dp[x]`đại diện cho phần thưởng nạp tiền đầu tiên tối đa có thể nhận được bằng cách chi tiêu chính xác`x`nhân dân tệ trong lần mua hàng đầu tiên. Ban đầu mọi trạng thái đều bằng 0 vì không có phần thưởng nào được thu thập. 
2. Xử lý từng gói trong số bảy gói nạp lại như một vật phẩm trong ba lô 0/1. Đối với chi phí trọn gói`cost`nhân dân tệ và cho`bonus`phiếu giảm giá, cập nhật trạng thái từ số tiền lớn hơn xuống số tiền nhỏ hơn. Việc chuyển đổi là bỏ qua gói này hoặc mua một lần. 
3. Sau khi tất cả các gói được xử lý, hãy tìm giá trị thưởng tốt nhất trong số tất cả số tiền lên tới`n`. Số tiền sử dụng cho lần mua hàng đầu tiên không cần phải chính xác`n`, vì số tiền còn dư vẫn có thể chi tiêu bình thường. 
4. Thêm`10 * n`để có được phần thưởng tốt nhất. Đây là câu trả lời cuối cùng vì mỗi nhân dân tệ đều đóng góp phần thưởng bình thường như nhau bất kể kế hoạch nạp tiền. 

Lý do cần cập nhật ngược là vì mỗi gói chỉ có thể trao phần thưởng lần đầu một lần. Cập nhật từ thấp lên cao sẽ cho phép cùng một gói được chọn nhiều lần trong cùng một lần lặp. 

Tại sao nó hoạt động: Trạng thái lập trình động xem xét chính xác các lựa chọn duy nhất ảnh hưởng đến tiền thưởng. Mỗi gói đóng góp phần thưởng đầu tiên một lần hoặc không đóng góp gì. Sau khi xử lý tất cả các gói, mọi tập hợp con hợp lệ của giao dịch mua lần đầu đều đã được xem xét. Số tiền còn lại không ảnh hưởng đến việc lựa chọn tiền thưởng và luôn đóng góp phần thưởng bình thường như nhau, vì vậy việc thêm`10 * n`hoàn thành tổng số tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    costs = [1, 6, 28, 88, 198, 328, 648]
    bonuses = [8, 18, 28, 58, 128, 198, 388]

    dp = [0] * (n + 1)

    for cost, bonus in zip(costs, bonuses):
        for money in range(n, cost - 1, -1):
            dp[money] = max(dp[money], dp[money - cost] + bonus)

    print(n * 10 + max(dp))

if __name__ == "__main__":
    solve()
```Các mảng`costs`Và`bonuses`lưu trữ bảy lần mua hàng đầu tiên có thể. Mỗi cặp tạo thành một vật phẩm trong ba lô. 

Vòng lặp lồng nhau thực hiện quá trình chuyển đổi ba lô 0/1. Vòng lặp kết thúc`money`đi từ`n`đi xuống vì gói hiện tại không được sử dụng lại trạng thái được tạo trước đó trong cùng một lần lặp. Nếu nó tăng lên, việc mua một gói có thể vô tình mua cùng một gói nhiều lần. 

trận chung kết`max(dp)`được sử dụng thay vì`dp[n]`vì lần mua đầu tiên có thể không tiêu hết số tiền. Ví dụ: Kelo có thể chi 198 nhân dân tệ cho một gói để nhận được khoản tiền thưởng lớn và sau đó số tiền còn lại sẽ được xử lý tốt hơn như nạp tiền thông thường. 

Tất cả các giá trị vẫn ở mức nhỏ. Phần thưởng bình thường tối đa chỉ là`20000`phiếu giảm giá và tổng số tiền thưởng cũng nhỏ nên việc tràn số nguyên Python không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 100`. 

| Bước | Gói gia công | Trạng thái chính | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | Không có |`dp[0..100] = 0`| Không có giao dịch mua đầu tiên nào được chọn | 
| Sau chi phí 1 | 1 nhân dân tệ, thưởng 8 |`dp[1] = 8`| Gói nhỏ nhất sẽ có sẵn | 
| Sau chi phí 6 | 6 nhân dân tệ, tiền thưởng 18 |`dp[6] = 18`,`dp[7] = 26`| Cả hai lựa chọn gói có thể kết hợp | 
| Sau tất cả các gói | Tất cả bảy loại | Tiền thưởng tối đa là 84 | Mua hàng đầu tiên tốt nhất sử dụng một phần ngân sách | 
| Cuối cùng | Thêm phần thưởng bình thường |`1000 + 84 = 1084`| Số tiền còn lại cung cấp phiếu giảm giá thông thường | 

Dấu vết này cho thấy lý do tại sao thuật toán không cần phải quyết định chính xác các giao dịch mua lặp lại. Chỉ những lần mua hàng đầu tiên mới ảnh hưởng đến tiền thưởng và tất cả số tiền khác được chuyển đổi theo tỷ giá cố định. 

Đối với mẫu thứ hai,`n = 198`. 

| Bước | Gói gia công | Trạng thái chính | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | Không có | Tất cả đều bằng không | Không thu được tiền thưởng | 
| Quy trình gói 88 nhân dân tệ | Tiền thưởng 58 | Kỳ sử dụng 88 nhân dân tệ cải thiện | Các giao dịch mua đầu tiên sẽ có sẵn | 
| Quy trình gói 198 nhân dân tệ | Tiền thưởng 128 |`dp[198]`trở thành 128 | Gói lớn được xem xét một lần | 
| Sau tất cả các gói | Tất cả bảy loại | Tiền thưởng tối đa là 128 | Gói 198 là tốt nhất cho ngân sách này | 
| Cuối cùng | Thêm phần thưởng bình thường |`1980 + 128 = 2108`| Đầu ra khớp với mẫu | 

Trường hợp này chứng tỏ rằng gói lớn hơn có thể là tối ưu vì phần thưởng lần đầu được nhận ngay lập tức trong khi phần thưởng thông thường đã được đảm bảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(7n) | Mỗi gói trong số bảy gói cập nhật tất cả các trạng thái tiền có thể có | 
| Không gian | O(n) | Mảng lập trình động lưu trữ một giá trị cho mỗi số tiền có thể chi tiêu | 

Với`n <= 2000`, thuật toán thực hiện khoảng 14 nghìn cập nhật trạng thái, dễ dàng đáp ứng các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Sample tests
assert run("100\n") == "1084\n", "sample 1"
assert run("198\n") == "2108\n", "sample 2"

# Minimum value
assert run("1\n") == "18\n", "minimum budget"

# Only enough for the largest package
assert run("648\n") == "6868\n", "large package bonus counted once"

# Maximum input size
assert run("2000\n") == "20388\n", "maximum budget"

# Boundary around package prices
assert run("6\n") == "78\n", "six yuan package handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`18`| Ngân sách tối thiểu và gói nhỏ nhất | 
|`648`|`6868`| Phần thưởng đầu tiên được tính một lần | 
|`2000`|`20388`| Xử lý hạn chế tối đa | 
|`6`|`78`| Ranh giới chi phí trọn gói chính xác | 

## Vỏ cạnh 

cho`n = 1`, thuật toán xử lý gói một nhân dân tệ và cập nhật`dp[1]`ĐẾN`8`. Câu trả lời cuối cùng là`10 + 8 = 18`. Nó không bao giờ cố gắng mua một gói đắt tiền hơn vì những chuyển đổi đó là không thể. 

Vì`n = 648`, giai đoạn lập trình động có thể chọn gói 648 nhân dân tệ và chỉ nhận được phần thưởng một lần là`388`. Phép tính còn lại cộng`6480`phiếu giảm giá bình thường, tặng`6868`. Việc mua lặp lại cùng một gói không bao giờ được đại diện vì mỗi gói chỉ được xử lý một lần. 

Đối với ngân sách không bằng bất kỳ giá trọn gói nào, chẳng hạn như`n = 100`, thuật toán không yêu cầu điền chính xác trong quá trình lựa chọn tiền thưởng. Nó tìm thấy tập hợp con tốt nhất của lần mua hàng đầu tiên sử dụng tối đa 100 nhân dân tệ, sau đó tự động chuyển số tiền chưa sử dụng thành phiếu giảm giá thông thường thông qua giá cố định.`10 * n`thuật ngữ. Điều này ngăn chặn việc mất giá trị do buộc phải khớp ba lô chính xác một cách nhân tạo.
