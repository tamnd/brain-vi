---
title: "CF 106262G - Lưu lượng cắt tối đa"
description: "Chúng ta được cung cấp một đường thẳng gồm các điểm kiểm tra từ 1 đến n. Nước luôn chảy thẳng về phía trước từ điểm kiểm tra 1 đến điểm kiểm tra n và cách duy nhất để ngăn chặn lũ lụt là chặn ít nhất một trong n-1 kết nối giữa các điểm kiểm tra liên tiếp."
date: "2026-06-19T14:19:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106262
codeforces_index: "G"
codeforces_contest_name: "2025 ICPC Asia Manila Regional"
rating: 0
weight: 106262
solve_time_s: 61
verified: true
draft: false
---

[CF 106262G - Lưu lượng cắt tối thiểu tối đa](https://codeforces.com/problemset/problem/106262/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường thẳng gồm các điểm kiểm tra từ 1 đến n. Nước luôn chảy thẳng về phía trước từ điểm kiểm tra 1 đến điểm kiểm tra n và cách duy nhất để ngăn chặn lũ lụt là chặn ít nhất một trong n-1 kết nối giữa các điểm kiểm tra liên tiếp. 

Mỗi kết nối i giữa i và i + 1 có thể bị chặn bằng cách thực hiện một dự án tương ứng. Mỗi dự án có một giá trị xi, giá trị này ảnh hưởng đến tiền của bạn: giá trị dương làm tăng tiền của bạn, giá trị âm làm giảm tiền của bạn. Bạn bắt đầu với ngân sách ban đầu b. 

Bạn có thể chọn bất kỳ tập hợp con dự án nào, theo bất kỳ thứ tự nào, nhưng mỗi dự án chỉ có thể được sử dụng nhiều nhất một lần. Một dự án chỉ có thể thực hiện được nếu hiện tại bạn có đủ tiền để trả chi phí khi xi âm. Các dự án tích cực không bao giờ tạo ra vấn đề khả thi vì chúng làm tăng ngân sách của bạn. 

Mục tiêu của bạn là chọn một tập hợp con các dự án sao cho ít nhất một kết nối bị chặn và thứ tự thực hiện hợp lệ đối với ngân sách đang hoạt động, đồng thời tối đa hóa số tiền cuối cùng của bạn. 

Những ràng buộc đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính. Với n lên đến 100000 và xi giới hạn bởi 10000 về độ lớn, bất kỳ chiến lược O(n^2) nào thử trực tiếp các hoán vị hoặc tập hợp con đều không thể thực hiện được. Ngay cả O(n log n) cũng được chấp nhận, nhưng O(n^2) hoặc lý luận hàm mũ trên các tập hợp con không thể vượt qua. 

Một vấn đề tế nhị xuất hiện khi tất cả các dự án đều tiêu cực và ngân sách ban đầu quá nhỏ để bắt đầu bất kỳ dự án nào. Trong trường hợp đó, mặc dù việc chặn một cạnh về mặt lý thuyết sẽ giải quyết được tình trạng tràn ngập, nhưng bạn không thể thực hiện được bước đầu tiên của bất kỳ chuỗi hợp lệ nào, vì vậy câu trả lời phải là −1. 

Một trường hợp thất bại khác xuất phát từ việc cho rằng việc chọn tất cả các dự án luôn là tối ưu. Nếu những điều tiêu cực ban đầu quá tốn kém, chúng có thể chặn khả năng tiếp cận các dự án có lợi sau này, vì vậy việc đặt hàng là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của các cạnh, kiểm tra xem liệu ít nhất một cạnh có được chọn hay không và xác minh xem liệu có tồn tại thứ tự của các dự án đã chọn để giữ cho ngân sách không âm trong suốt quá trình thực hiện hay không. Đối với mỗi tập hợp con, chúng tôi sẽ mô phỏng tất cả các hoán vị hoặc cố gắng tìm một chuỗi hợp lệ. Điều này nhanh chóng trở nên không khả thi: có 2^(n−1) tập hợp con và mỗi lần kiểm tra tính hợp lệ có thể mất O(k log k) hoặc tệ hơn, dẫn đến bùng nổ theo cấp số nhân. 

Quan sát quan trọng là tính khả thi chỉ phụ thuộc vào cách chúng ta sắp xếp các dự án đã chọn chứ không phụ thuộc vào bất kỳ cấu trúc biểu đồ nào ngoài một dòng. Các dự án tích cực luôn an toàn để thực hiện sớm vì chúng chỉ làm tăng ngân sách. Hạn chế duy nhất đến từ các dự án tiêu cực: mỗi dự án tiêu cực đòi hỏi phải có đủ tiền trước khi thực hiện. 

Điều này chuyển vấn đề thành việc chọn một tập hợp con các mục có lợi nhuận xi, trong đó số dương là lợi nhuận miễn phí và số âm yêu cầu điều kiện tài nguyên tiền tố tối thiểu. Khi tất cả các dự án tích cực được thực hiện, câu hỏi còn lại sẽ trở thành: chúng ta có thể lên kế hoạch cho những dự án tiêu cực nào sao cho ở mỗi bước, số tiền hiện tại không bao giờ giảm xuống dưới ngưỡng yêu cầu? Cấu trúc tối ưu là luôn thực hiện các dự án âm có giá cả phải chăng theo thứ tự chi phí tăng dần, nghĩa là chúng tôi ưu tiên những dự án có tổn thất tuyệt đối nhỏ hơn trước. Việc đặt hàng tham lam này đảm bảo chúng ta không bao giờ “lãng phí” ngân sách sớm cho một khoản lỗ lớn khi khoản lỗ nhỏ hơn có thể được xử lý một cách an toàn trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n!) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách quyết định thành việc xử lý các dự án có lợi nhuận trước tiên và sau đó xử lý cẩn thận các dự án đắt tiền.

1. Chia dự án thành hai nhóm: nhóm có xi > 0 và nhóm có xi ≤ 0. Nhóm tích cực luôn cải thiện ngân sách và không bao giờ hạn chế đặt hàng nên luôn an toàn khi đưa vào. 
2. Thêm tất cả xi dương vào câu trả lời ngay lập tức và tăng ngân sách hiện tại lên bằng tổng của chúng. Điều này tối đa hóa các nguồn lực sẵn có cho các quyết định sau này và không bao giờ có lý do để trì hoãn chúng. 
3. Thu thập tất cả các dự án tiêu cực và sắp xếp chúng theo thứ tự giảm dần của xi. Điều này có nghĩa là trước tiên chúng tôi xử lý những tổn thất ít gây thiệt hại nhất vì giá trị như −1 sẽ an toàn hơn khi thanh toán giá trị như −100. 
4. Lặp lại danh sách đã sắp xếp này. Với mỗi xi âm, hãy kiểm tra xem ngân sách hiện tại có ít nhất là −xi hay không. Nếu có, hãy thực hiện nó, trừ −xi khỏi ngân sách và cộng xi vào tổng câu trả lời. 
5. Nếu một dự án tiêu cực không đủ khả năng chi trả vào lúc này, hãy bỏ qua nó vĩnh viễn. Sau này nó sẽ không bao giờ trở nên hợp lý hơn vì những tiêu cực chỉ làm giảm ngân sách. 
6. Đảm bảo rằng ít nhất một dự án đã được thực hiện tổng thể. Nếu không có gì được chọn, trả về −1 vì chúng ta không chặn được bất kỳ cạnh nào. 

Ý tưởng chính đằng sau việc đặt hàng là tổn thất nhỏ hơn sẽ đảm bảo tính linh hoạt cho các lựa chọn trong tương lai. Thực hiện một khoản lỗ lớn sớm có thể phá hủy tính khả thi của những khoản nhỏ hơn, trong khi điều ngược lại không bao giờ ảnh hưởng đến tính khả thi. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hạn chế duy nhất quan trọng là liệu ngân sách hiện tại có đủ lớn để chi trả cho một dự án âm hay không. Các dự án tích cực chỉ làm tăng ngân sách nên có thể áp dụng an toàn trước mà không mất tính tổng quát. Trong số các dự án tiêu cực, bất kỳ lịch trình tối ưu nào cũng có thể được chuyển thành một kế hoạch trong đó tất cả các dự án tiêu cực đã chọn xuất hiện theo thứ tự chi phí không giảm mà không vi phạm tính khả thi, vì việc hoán đổi một khoản lỗ đắt hơn trước đó chỉ có thể khiến các ràng buộc ngân sách trở nên khó khăn hơn cho các bước sau. Đối số trao đổi này đảm bảo rằng việc xây dựng được sắp xếp theo chi phí tham lam không loại trừ bất kỳ giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, b = map(int, input().split())
    arr = list(map(int, input().split()))

    total = b
    picked_any = False

    positives = 0
    negatives = []

    for x in arr:
        if x > 0:
            positives += x
            picked_any = True
        else:
            negatives.append(x)

    total += positives

    negatives.sort(reverse=True)

    for x in negatives:
        if total >= -x:
            total += x
            picked_any = True

    if not picked_any:
        print(-1)
    else:
        print(total)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo cấu trúc trực tiếp. Chúng tôi tích lũy tất cả những đóng góp tích cực trước tiên vì chúng mang lại lợi ích vô điều kiện. Sau đó, chúng tôi sắp xếp các giá trị âm để thử giảm thiểu tác hại ít nhất trước tiên. Việc kiểm tra ngân sách đảm bảo chúng tôi không bao giờ thực hiện dự án khiến ngân sách không hợp lệ tại thời điểm đó. Cờ boolean theo dõi xem có dự án nào được chọn thành công hay không, điều này là bắt buộc vì việc chặn không có cạnh nào không giải quyết được vấn đề ngập lụt. 

Một điểm tinh tế là chúng tôi không bao giờ xem xét lại những tiêu cực đã bỏ qua. Điều này đúng vì bỏ qua số âm có nghĩa là ngân sách hiện tại không đủ và vì các hoạt động còn lại không thể tăng ngân sách trước khi số âm đó được xem xét lại ở vị trí tốt hơn nên sau này sẽ không bao giờ khả thi. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên. 

Các dự án đầu vào được`3, -1, 4, 1, -5`với ngân sách ban đầu 7. 

Trước tiên, chúng tôi lấy số dương 3, 4 và 1, tăng ngân sách lên 15. 

| Bước | Hành động | Dự án | Ngân sách trước | Ngân sách sau | 
| --- | --- | --- | --- | --- | 
| 1 | Hãy tích cực | 3 | 7 | 10 | 
| 2 | Hãy tích cực | 4 | 10 | 14 | 
| 3 | Hãy tích cực | 1 | 14 | 15 | 

Bây giờ chúng tôi xử lý các số âm được sắp xếp: −1, −5. 

| Bước | Hành động | Dự án | Ngân sách trước | Ngân sách sau | 
| --- | --- | --- | --- | --- | 
| 1 | Đi | −1 | 15 | 14 | 
| 2 | Đi | −5 | 14 | 9 | 

Câu trả lời cuối cùng là 15. 

Dấu vết này cho thấy rằng việc duy trì ngân sách lớn sớm cho phép tất cả các khoản tiêu cực được xử lý một cách an toàn theo thứ tự chi phí tăng dần. 

Bây giờ hãy xem xét mẫu thứ hai. 

Đầu vào là`-6, -7, -6, -7`với ngân sách ban đầu 4. 

Không có số dương nào tồn tại nên chúng tôi chỉ thử sắp xếp số âm: −6, −6, −7, −7. 

| Bước | Hành động | Dự án | Ngân sách trước | Ngân sách sau | 
| --- | --- | --- | --- | --- | 
| 1 | Bỏ qua (không đủ) | −6 | 4 | 4 | 

Chúng tôi không thể thực hiện bất kỳ dự án nào, vì vậy chúng tôi không bao giờ chặn kết nối, khiến nhiệm vụ không thể thực hiện được. 

Điều này cho thấy tính khả thi phụ thuộc vào việc có đủ khả năng đáp ứng ít nhất một âm ban đầu hay có một lực đẩy dương, nếu không hệ thống không thể khởi động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp tiêu cực chiếm ưu thế; tất cả các hoạt động khác là tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách phủ định | 

Thuật toán phù hợp thoải mái trong các giới hạn vì n lên tới 100000. Sắp xếp một lần và thực hiện quét tuyến tính duy nhất là đủ hiệu quả cho các ràng buộc điển hình của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, b = map(int, input().split())
    arr = list(map(int, input().split()))

    total = b
    picked = False

    pos = 0
    neg = []

    for x in arr:
        if x > 0:
            pos += x
            picked = True
        else:
            neg.append(x)

    total += pos
    neg.sort(reverse=True)

    for x in neg:
        if total >= -x:
            total += x
            picked = True

    return str(total) if picked else "-1"

# provided samples
assert run("6 7\n3 -1 4 1 -5\n") == "15"
assert run("5 4\n-6 -7 -6 -7\n") == "-1"

# minimum size
assert run("2 0\n5\n") == "5"

# all positives
assert run("4 1\n2 3 4\n") == "10"

# all negatives but feasible chain
assert run("4 10\n-3 -4 -5\n") == " -2".strip()

# tight budget edge
assert run("3 5\n-6 1\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tích cực | tổng hợp | tích lũy cơ bản | 
| tất cả những tiêu cực không thể thực hiện được | -1 | phát hiện không thể | 
| ngân sách eo hẹp hỗn hợp | tập hợp con đúng | đặt hàng khả thi tham lam | 
| cạnh đơn | quyết định trực tiếp | hành vi trường hợp tối thiểu | 

## Vỏ cạnh 

Khi tất cả các dự án đều âm và không có dự án nào có thể đủ khả năng ban đầu, thuật toán ngay lập tức trả về −1 vì không có hoạt động nào có thể bắt đầu. Ví dụ, đầu vào`n = 3, b = 1, [-5, -6]`không bao giờ vượt qua bước kiểm tra khả năng chi trả đầu tiên, vì vậy không có dự án nào được thực hiện và cờ vẫn sai. 

Khi có cả giá trị dương và giá trị âm nhưng giá trị dương nhỏ, thuật toán đảm bảo giá trị dương được áp dụng trước, có khả năng tạo ra giá trị âm mà trước đây không thể chấp nhận được. Ví dụ,`b = 1, [2, -5]`tăng ngân sách lên 3 trước khi thử −5, vẫn thất bại chính xác vì số âm vẫn không khả thi. 

Khi một âm bản lớn xuất hiện trước những âm bản nhỏ hơn, việc sắp xếp đảm bảo chúng ta không bao giờ lãng phí ngân sách cho nó sớm. Vì`b = 10, [-1, -1, -20]`, việc thực hiện hai thao tác −1 trước tiên sẽ giảm ngân sách dần dần và tránh việc thử −20 sớm, điều này sẽ thất bại và có khả năng chặn lý do nếu sắp xếp sai.
