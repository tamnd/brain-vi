---
title: "CF 102760H - Tiếp thị cạnh tranh mô phỏng"
description: "Vấn đề mô hình một chuỗi các cuộc đấu giá quảng cáo. Chỉ có sáu loại quảng cáo có thể. Mỗi loại có một mức giá cố định và công ty có ngân sách hạn chế trước khi cuộc đấu giá bắt đầu."
date: "2026-07-30T04:09:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "H"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 77
verified: true
draft: false
---

[CF 102760H - Tiếp thị cạnh tranh mô phỏng](https://codeforces.com/problemset/problem/102760/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô hình một chuỗi các cuộc đấu giá quảng cáo. Chỉ có sáu loại quảng cáo có thể. Mỗi loại có một mức giá cố định và công ty có ngân sách hạn chế trước khi cuộc đấu giá bắt đầu. Trước khi bất kỳ cuộc đấu giá nào diễn ra, công ty phải quyết định loại quảng cáo nào họ sẵn sàng đấu giá. 

Khi cuộc đấu giá xuất hiện, công ty chỉ đặt giá thầu nếu loại hình đấu giá đã được chọn trước và ngân sách còn lại đủ để trả giá cho loại hình đó. Nếu nó đặt giá thầu, nó sẽ thắng ngay lập tức, trả giá và nhận được một quảng cáo được hiển thị. Mục tiêu là chọn tập hợp các loại sao cho tổng số lần đấu giá thành công càng lớn càng tốt. 

Dữ liệu đầu vào chứa số lượng phiên đấu giá, ngân sách ban đầu, sáu mức giá quảng cáo và loại mỗi phiên đấu giá theo thứ tự thời gian. Đầu ra là số lượng quảng cáo tối đa có thể nhận được bằng cách chọn tập hợp loại quảng cáo tốt nhất có thể. 

Số lượng đấu giá có thể lên tới 100.000, do đó, một thuật toán thực hiện nhiều lần công việc tốn kém trên toàn bộ chuỗi chỉ được chấp nhận nếu số lần lặp lại rất nhỏ. Trong trường hợp xấu nhất, một giải pháp bậc hai sẽ yêu cầu khoảng 10 tỷ phép tính, vượt xa những gì thực tế. Ngân sách có thể lớn tới 10^9, do đó việc triển khai cũng cần số học số nguyên để xử lý số dư lớn còn lại một cách an toàn. 

Số lượng nhỏ các loại quảng cáo là hạn chế chính. Vì chỉ có sáu loại nên số lượng tập hợp loại có thể có chỉ là 2^6 = 64. Điều này thay đổi hoàn toàn cách tiếp cận vì việc kiểm tra mọi quyết định có thể là khả thi. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai bất cẩn. Nếu ngân sách nhỏ hơn mọi giá quảng cáo thì không có giá thầu nào có thể xảy ra. Ví dụ:```
1 0
5 6 7 8 9 10
1
```Đầu ra đúng là:```
0
```Giải pháp tính các loại quảng cáo đã chọn thay vì giá thầu thành công sẽ trả về giá trị dương không chính xác. 

Một trường hợp khó khăn khác là khi công ty chọn một loại nhưng chỉ có đủ khả năng chi trả cho một số lần xuất hiện của loại đó. Ví dụ:```
4 5
3 4 5 6 7 8
1 1 1 1
```Đầu ra đúng là:```
1
```Lần đấu giá đầu tiên có thể thắng vì giá là 3. Sau khi tiêu số tiền đó, ngân sách còn lại là 2 nên mọi cuộc đấu giá cùng loại sau đó đều phải bỏ qua. Việc thực hiện bất cẩn có thể tính cả bốn lần xuất hiện. 

Trường hợp cuối cùng là khi bỏ qua loại giá rẻ sẽ cho phép thực hiện một chuỗi mua hàng có giá trị hơn. Ví dụ:```
5 10
2 10 1 100 100 100
1 2 1 2 1
```Sản lượng tối ưu là:```
3
```Loại được chọn là loại 1 và loại 2. Công ty mua loại 1, sau đó mua loại 2, rồi lại loại 1. Thứ tự của các cuộc đấu giá rất quan trọng, vì vậy các giải pháp chỉ xem xét tổng tần số mà không mô phỏng trình tự sẽ thất bại. 

## Phương pháp tiếp cận 

Ý tưởng đầu tiên là thử mọi loại quảng cáo có thể. Đối với mỗi bộ, chúng tôi mô phỏng các cuộc đấu giá từ trái sang phải. Trong quá trình mô phỏng, chúng tôi giữ số tiền hiện tại và bất cứ khi nào loại đấu giá hiện tại thuộc về nhóm đã chọn và chúng tôi có đủ khả năng chi trả, chúng tôi sẽ tiêu tiền và tăng câu trả lời. 

Cách tiếp cận bạo lực này thực sự là cách tiếp cận tối ưu vì không gian tìm kiếm rất nhỏ. Nếu có nhiều loại quảng cáo thì việc kiểm tra từng tập hợp con sẽ là không thể. Với sáu loại, chỉ có 64 tập hợp con. Mỗi mô phỏng mất O(N), do đó tổng công việc chỉ là 64 × N hoạt động. Với N = 100.000, đây là khoảng 6,4 triệu séc đấu giá, có thể dễ dàng quản lý được. 

Quan sát mở ra giải pháp là quyết định không phải là chọn các cuộc đấu giá riêng lẻ. Khi một loại được chọn, mọi lần xuất hiện trong tương lai của loại đó đều được xử lý theo cùng một quy tắc. Vì chỉ có sáu lựa chọn độc lập nên không gian quyết định hoàn chỉnh nằm gọn trong mặt nạ bit. 

Cách tiếp cận bạo lực có hiệu quả vì số lượng chiến lược khả thi là ít. Sẽ thất bại nếu số lượng loại quảng cáo tăng lên, nhưng vấn đề đưa ra chính xác cấu trúc cần thiết để liệt kê trực tiếp tất cả các chiến lược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các tập hợp con kiểu | O(2^6 × N) | O(1) | Đã chấp nhận | 
| Liệt kê tập hợp con tối ưu với mô phỏng bitmask | O(2^6 × N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi tập hợp loại quảng cáo được chọn có thể có như một mặt nạ sáu bit. Bit i được thiết lập khi loại quảng cáo i được đưa vào chiến lược. Chỉ có 64 mặt nạ nên chúng tôi có thể thử nghiệm mọi chiến lược khả thi. 
2. Đối với mỗi mặt nạ, hãy đặt lại ngân sách còn lại về giá trị ban đầu và bắt đầu quét các phiên đấu giá từ đầu. Mỗi mặt nạ thể hiện một quyết định hoàn chỉnh được đưa ra trước khi cuộc đấu giá bắt đầu, vì vậy quá trình mô phỏng phải luôn bắt đầu với ngân sách ban đầu. 
3. Khi xử lý phiên đấu giá, hãy kiểm tra xem loại quảng cáo của nó có được bật trong mặt nạ hiện tại hay không. Nếu nó không được kích hoạt, hãy bỏ qua cuộc đấu giá vì công ty đã quyết định trước chuỗi đấu giá rằng sẽ không bao giờ đấu giá loại hình này. 
4. Nếu loại được bật, hãy so sánh ngân sách còn lại với giá của loại đó. Nếu công ty có đủ khả năng, hãy trừ giá và tăng số lượng giá thầu thành công. Nếu không, hãy bỏ qua cuộc đấu giá này. 
5. Sau khi kết thúc chuỗi, hãy so sánh số lượng giá thầu thành công từ mặt nạ này với câu trả lời tốt nhất được tìm thấy cho đến nay. Giá trị lớn nhất trên tất cả các mặt nạ là kết quả được yêu cầu. 

Lý do điều này có tác dụng là vì mọi chiến lược hợp lệ đều tương ứng với chính xác một tập hợp con trong sáu loại quảng cáo. Thuật toán kiểm tra mọi tập hợp con có thể và mô phỏng cho từng tập hợp con tuân theo các quy tắc đặt giá thầu chính xác của bài toán. Vì một trong các tập con đó là tối ưu nên giá trị lớn nhất tìm được phải là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    price = list(map(int, input().split()))
    auctions = [int(x) - 1 for x in input().split()]

    ans = 0

    for mask in range(1 << 6):
        money = K
        count = 0

        for t in auctions:
            if (mask >> t) & 1 and money >= price[t]:
                money -= price[t]
                count += 1

        if count > ans:
            ans = count

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình đại diện cho các loại quảng cáo đã chọn bằng mặt nạ số nguyên. biểu hiện`(mask >> t) & 1`kiểm tra xem loại đấu giá hiện tại có được đưa vào chiến lược hay không. 

Đối với mỗi mặt nạ, các biến mô phỏng được đặt lại. Ngân sách phải được sao chép thay vì sử dụng lại vì mọi chiến lược đều bắt đầu trước khi bất kỳ cuộc đấu giá nào diễn ra. 

Các loại đấu giá được chuyển đổi từ lập chỉ mục dựa trên một sang lập chỉ mục dựa trên 0 ngay sau khi đọc. Điều này tránh việc trừ liên tục một trong khi xử lý chuỗi. 

Không có lo ngại về tràn trong Python vì số nguyên tự động tăng lên khi cần thiết. Chi tiết thực hiện chính là giữ trật tự đấu giá. Việc sắp xếp hoặc nhóm các cuộc đấu giá sẽ thay đổi quy trình vì các giao dịch mua trước đó sẽ ảnh hưởng đến việc liệu giá thầu sau này có phải chăng hay không. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
6 10
1 2 3 4 5 6
6 5 4 3 2 1
```Một mặt nạ tối ưu chọn loại từ 1 đến 4. 

| Đấu giá | Loại | Ngân sách còn lại | Hành động | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 10 | Bỏ qua | 0 | 
| 2 | 5 | 10 | Bỏ qua | 0 | 
| 3 | 4 | 10 | Mua, chi 4 | 1 | 
| 4 | 3 | 6 | Mua, dành 3 | 2 | 
| 5 | 2 | 3 | Mua, chi 2 | 3 | 
| 6 | 1 | 1 | Không đủ khả năng | 3 | 

Dấu vết này cho thấy tại sao giải pháp phải mô phỏng trình tự. Các loại được chọn giống nhau có thể hoạt động khác nhau tùy thuộc vào thời điểm các cuộc đấu giá đắt tiền xuất hiện. 

Đối với ví dụ thứ hai:```
12 10
1 1 2 2 3 3
6 5 4 3 2 1 1 2 3 4 5 6
```Việc chọn loại 1, 2, 3 và 4 sẽ đưa ra hành vi sau. 

| Đấu giá | Loại | Ngân sách còn lại | Hành động | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 10 | Bỏ qua | 0 | 
| 2 | 5 | 10 | Bỏ qua | 0 | 
| 3 | 4 | 10 | Mua | 1 | 
| 4 | 3 | 6 | Mua | 2 | 
| 5 | 2 | 3 | Mua | 3 | 
| 6 | 1 | 1 | Mua | 4 | 
| 7 | 1 | 0 | Không đủ khả năng | 4 | 
| 8 | 2 | 0 | Không đủ khả năng | 4 | 
| 9 | 3 | 0 | Không đủ khả năng | 4 | 
| 10 | 4 | 0 | Không đủ khả năng | 4 | 
| 11 | 5 | 0 | Bỏ qua | 4 | 
| 12 | 6 | 0 | Bỏ qua | 4 | 

Dấu vết chứng tỏ rằng chiến lược được đánh giá theo dòng thời gian hoàn chỉnh. Loại được chọn không đảm bảo rằng mọi lần xuất hiện sẽ được mua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^6 × N) | Có 64 bộ loại có thể có và mỗi bộ yêu cầu một lần vượt qua các cuộc đấu giá. | 
| Không gian | O(1) | Chỉ có mặt nạ, ngân sách và bộ đếm hiện tại được lưu trữ. | 

Với tối đa 100.000 cuộc đấu giá, thuật toán thực hiện khoảng 6,4 triệu lượt kiểm tra. Hệ số không đổi nhỏ vì mỗi lần kiểm tra chỉ bao gồm một thao tác bit và so sánh. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    N, K = map(int, input().split())
    price = list(map(int, input().split()))
    auctions = [int(x) - 1 for x in input().split()]

    ans = 0
    for mask in range(64):
        money = K
        count = 0
        for t in auctions:
            if ((mask >> t) & 1) and money >= price[t]:
                money -= price[t]
                count += 1
        ans = max(ans, count)

    sys.stdin = old_stdin
    return str(ans)

assert solve_data("""6 10
1 2 3 4 5 6
6 5 4 3 2 1
""") == "4", "sample 1"

assert solve_data("""12 10
1 1 2 2 3 3
6 5 4 3 2 1 1 2 3 4 5 6
""") == "7", "sample 2"

assert solve_data("""1 0
5 6 7 8 9 10
1
""") == "0", "no budget"

assert solve_data("""4 5
3 4 5 6 7 8
1 1 1 1
""") == "1", "repeated expensive purchases"

assert solve_data("""5 10
2 10 1 100 100 100
1 2 1 2 1
""") == "3", "order dependency"

assert solve_data("""100000 1000000000
1 1 1 1 1 1
""" + "1 " * 99999 + "1\n") == "100000", "large input size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có ngân sách với các lựa chọn có vẻ rẻ tiền | 0 | Thuật toán chỉ tính giá thầu thành công. | 
| Cùng loại xuất hiện nhiều lần | 1 | Ngân sách giảm sau khi mua và không thể tái sử dụng. | 
| Chuỗi đắt và rẻ hỗn hợp | 3 | Lệnh đấu giá ảnh hưởng đến kết quả. | 
| Trình tự rất lớn | 100000 | Cách tiếp cận O(64N) xử lý kích thước đầu vào tối đa. | 

## Vỏ cạnh 

Khi ngân sách bằng 0, mọi cuộc đấu giá đều phải bị bỏ qua vì mọi giá quảng cáo đều dương. Đối với đầu vào:```
1 0
5 6 7 8 9 10
1
```mọi mô phỏng mặt nạ đều giữ ngân sách ở mức 0, do đó không thể mua hàng và câu trả lời vẫn là 0. 

Khi một loại quảng cáo xuất hiện liên tục, thuật toán sẽ ngừng mua sau khi hết ngân sách. Vì:```
4 5
3 4 5 6 7 8
1 1 1 1
```mặt nạ tốt nhất bao gồm loại 1. Cuộc đấu giá đầu tiên chi 3 đô la, để lại 2 đô la. Ba cuộc đấu giá tiếp theo đều thất bại vì ngân sách còn lại không đủ. Câu trả lời là một. 

Khi nhiều loại cạnh tranh vì số tiền hạn chế, việc liệt kê tập hợp con sẽ kiểm tra tất cả các sự cân bằng có thể xảy ra. Vì:```
5 10
2 10 1 100 100 100
1 2 1 2 1
```mặt nạ chứa loại 1 và 2 tạo ra ba lần mua thành công. Mô phỏng cho thấy những thay đổi ngân sách giống hệt như sẽ xảy ra theo thứ tự đấu giá thực, do đó, nó không nhầm lẫn tổng số lượng với số lượng có thể đạt được.
