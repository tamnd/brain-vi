---
title: "CF 104598A - Chia dữ liệu"
description: "Chúng tôi được cung cấp một tập hợp các tệp, mỗi tệp có kích thước dương được đo bằng bit và ngân sách lưu trữ giới hạn tổng số bit chúng tôi có thể phân bổ."
date: "2026-06-30T03:03:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104598
codeforces_index: "A"
codeforces_contest_name: "GPL 2023 Advanced"
rating: 0
weight: 104598
solve_time_s: 61
verified: true
draft: false
---

[CF 104598A - Chia dữ liệu](https://codeforces.com/problemset/problem/104598/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các tệp, mỗi tệp có kích thước dương được đo bằng bit và ngân sách lưu trữ giới hạn tổng số bit chúng tôi có thể phân bổ. Nhiệm vụ không phải là chọn các kết hợp tùy ý để có kích thước được lưu trữ tối đa mà thay vào đó là tối đa hóa số lượng tệp chúng tôi quản lý để đưa vào mà không vượt quá tổng dung lượng lưu trữ được phép. 

Nói cách khác, mỗi tệp là một mục có chi phí và chúng tôi muốn chọn càng nhiều mục càng tốt sao cho tổng chi phí của chúng không vượt quá khả năng cố định. Đầu ra là một số duy nhất: số lượng tệp lớn nhất có thể phù hợp với giới hạn lưu trữ. 

Các ràng buộc cho phép tối đa 100.000 tệp, với mỗi kích thước và dung lượng lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các tập hợp con hoặc thậm chí xem xét kết hợp. Tìm kiếm tập hợp con mạnh mẽ sẽ bao gồm 2^N khả năng, vượt xa tính khả thi. Ngay cả bất kỳ phương pháp bậc hai nào quét liên tục các phần tử còn lại cũng trở nên quá chậm khi N lớn. 

Áp lực tính toán quan trọng ở đây là chúng ta cần thứ gì đó gần với O(N log N) hoặc O(N), vì O(N^2) đã có rủi ro khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Một vấn đề tinh tế có thể đánh lừa cách tiếp cận ngây thơ là cho rằng việc chọn các tệp tùy ý hoặc xử lý chúng theo thứ tự đầu vào là đủ. Ví dụ: nếu chúng ta tham lam lấy những tệp đầu tiên xuất hiện, chúng ta có thể sớm lãng phí dung lượng và chặn các lựa chọn tối ưu hơn sau này. 

Hãy xem xét đầu vào này:```
N = 3, X = 10
sizes = [8, 1, 1]
```Nếu lấy theo thứ tự đầu vào thì lấy 8 trước, để lại dung lượng 2, sau đó lấy cả 1, cho ra 3 file. Điều này hoạt động ở đây, nhưng nếu đơn đặt hàng là`[8, 9, 1]`, lấy 8 lá đầu tiên còn 2 nên ta chỉ lấy được tổng cộng 1 file, mặc dù lấy`1 + 8`vẫn chỉ có 2 tệp, nhưng có thể sẽ áp dụng chiến lược tốt hơn`1 + 8`hoặc chỉ là hai cái nhỏ nhất tùy thuộc vào cấu trúc. Vấn đề thực sự là thứ tự đầu vào không có mối quan hệ nào với mức tối ưu. 

Một chế độ lỗi khác xuất hiện khi các tệp lớn xuất hiện sớm và cách tiếp cận tham lam "làm trong khi có thể" sẽ chặn các tệp nhỏ hơn sau đó. 

Chiến lược đúng phải bỏ qua hoàn toàn thứ tự đầu vào. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: thử mọi tập hợp con của tệp, tính tổng kích thước của nó và theo dõi kích thước tối đa của tập hợp con có tổng không vượt quá X. Điều này đúng vì nó đánh giá trực tiếp tất cả các khả năng, nhưng nó yêu cầu kiểm tra 2^N tập hợp con. Ngay cả đối với N = 40, điều này đã trở thành ranh giới và đối với N = 100.000 thì điều đó là không thể. 

Một biến thể bạo lực có cấu trúc hơn có thể thử tất cả các kết hợp của k tệp cho mỗi k từ 1 đến N, nhưng điều này vẫn bùng nổ về mặt tổ hợp. 

Quan sát quan trọng là chúng tôi không cố gắng tối đa hóa giá trị, chỉ đếm và tất cả các mục đều giống nhau về đóng góp giá trị. Điều này loại bỏ mọi sự đánh đổi giữa quy mô và lợi ích. Chiến lược tốt nhất là luôn ưu tiên các tệp nhỏ hơn trước tiên vì chúng tiêu tốn ít ngân sách hơn trên mỗi đơn vị số lượng thu được. 

Sau khi sắp xếp kích thước tệp theo thứ tự không giảm, chúng ta có thể tham lam lấy các tệp từ nhỏ nhất đến lớn nhất, tích lũy kích thước của chúng cho đến khi việc thêm tệp tiếp theo sẽ vượt quá giới hạn. Điều này hiệu quả vì bất kỳ giải pháp nào bao gồm tệp lớn hơn nhưng loại trừ tệp nhỏ hơn đều có thể được cải thiện bằng cách hoán đổi chúng mà không làm giảm tính khả thi trong khi có khả năng cho phép nhiều mục hơn về tổng thể. 

Do đó, vấn đề giảm xuống còn việc sắp xếp và quét tuyến tính đơn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N · N) | O(N) | Quá chậm | 
| Tối ưu (sắp xếp + quét tham lam) | O(N log N) | O(1) hoặc O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Sắp xếp tất cả kích thước tệp theo thứ tự không giảm. Điều này sắp xếp lại vấn đề để chúng tôi luôn xem xét các tệp rẻ nhất trước tiên về mặt chi phí lưu trữ. Điều này rất cần thiết vì thứ tự đầu vào không chứa ý nghĩa cấu trúc. 
2. Khởi tạo hai biến, một biến cho bộ nhớ được sử dụng hiện tại và một biến cho số lượng tệp đã chọn. Cả hai đều bắt đầu từ con số 0. 
3. Lặp lại danh sách các kích thước tệp được sắp xếp từ nhỏ nhất đến lớn nhất. Tại mỗi tệp, hãy kiểm tra xem việc thêm kích thước của nó có vượt quá giới hạn lưu trữ X hay không. 
4. Nếu tệp vừa, hãy đưa nó vào bằng cách cộng kích thước của nó vào tổng số hiện tại và tăng số lượng. Nếu nó không vừa, hãy dừng ngay lập tức, vì tất cả các tệp tiếp theo đều bằng hoặc lớn hơn và do đó cũng không thể vừa. 
5. Trả về số đếm cuối cùng. 

Điều kiện dừng sớm là hợp lý vì sau khi sắp xếp, tất cả các phần tử còn lại ít nhất phải lớn bằng phần tử hiện tại. Nếu cái hiện tại không vừa thì cái còn lại cũng không vừa. 

### Tại sao nó hoạt động 

Thuật toán duy trì đặc tính là ở mỗi bước, chúng tôi đã chọn tập k tệp nhỏ nhất có thể để đạt được tổng kích thước tối thiểu trong số tất cả các tập hợp con có kích thước k. Bất kỳ sai lệch nào thay thế tệp đã chọn nhỏ hơn bằng tệp lớn hơn chưa được chọn đều không thể giảm tổng kích thước, do đó, nó không thể tăng số lượng phần tử có thể chọn trong ngân sách cố định. Đối số trao đổi này đảm bảo rằng việc lựa chọn tiền tố tham lam sau khi sắp xếp là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, X = map(int, input().split())
    sizes = [int(input()) for _ in range(N)]
    
    sizes.sort()
    
    used = 0
    count = 0
    
    for s in sizes:
        if used + s <= X:
            used += s
            count += 1
        else:
            break
    
    print(count)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các kích thước tệp vào một danh sách và sắp xếp chúng theo thứ tự tăng dần để các tệp nhỏ nhất được xem xét trước tiên. Sau đó, vòng lặp tham lam sẽ tích lũy kích thước trong khi theo dõi số lượng tệp được bao gồm. Chi tiết quan trọng là câu lệnh break: khi một tệp không vừa, chúng tôi sẽ dừng ngay lập tức vì thứ tự sắp xếp đảm bảo không có tệp nào sau này có thể vừa. 

Một lỗi triển khai phổ biến là tiếp tục vòng lặp ngay cả khi đã vượt quá giới hạn và cố gắng bỏ qua các mục. Điều đó là không cần thiết và có nguy cơ tính toán logic không chính xác. Một sai lầm nhỏ khác là quên sắp xếp, điều này phá vỡ hoàn toàn tính tối ưu tham lam. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 34
14
25
47
11
6
```Kích thước được sắp xếp:```
[6, 11, 14, 25, 47]
```| Bước | Tệp hiện tại | Đã sử dụng trước đây | Được sử dụng sau | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 0 | 6 | 1 | 
| 2 | 11 | 6 | 17 | 2 | 
| 3 | 14 | 17 | 31 | 3 | 
| 4 | 25 | 31 | 31 | 3 (kích hoạt điều kiện dừng) | 

Tệp thứ tư không vừa và cũng không có tệp nào sau này có thể vừa. Câu trả lời là 3. 

### Mẫu 2 

đầu vào:```
6 18
2
5
6
4
13
1
```Kích thước được sắp xếp:```
[1, 2, 4, 5, 6, 13]
```| Bước | Tệp hiện tại | Đã sử dụng trước đây | Được sử dụng sau | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 3 | 2 | 
| 3 | 4 | 3 | 7 | 3 | 
| 4 | 5 | 7 | 12 | 4 | 
| 5 | 6 | 12 | 18 | 5 | 
| 6 | 13 | 18 | 18 | 5 (dừng) | 

Điều này chứng tỏ rằng việc lấy nhiều tệp nhỏ trước tiên sẽ tối đa hóa số lượng theo ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; quá trình quét là tuyến tính | 
| Không gian | O(N) | Lưu trữ danh sách đầu vào | 

Với N lên đến 100.000, việc sắp xếp và một lần chuyển dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ là tuyến tính theo số lượng tệp, cũng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    N, X = map(int, input().split())
    sizes = [int(input()) for _ in range(N)]
    sizes.sort()
    used = 0
    count = 0
    for s in sizes:
        if used + s <= X:
            used += s
            count += 1
        else:
            break
    print(count)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""5 34
14
25
47
11
6
""") == "3", "sample 1"

assert run("""6 18
2
5
6
4
13
1
""") == "5", "sample 2"

# custom cases
assert run("""1 10
5
""") == "1", "single file fits"
assert run("""1 3
5
""") == "0", "single file does not fit"
assert run("""4 10
8
7
6
5
""") == "1", "only smallest possible"
assert run("""5 100
1
1
1
1
1
""") == "5", "all fit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn phù hợp | 1 | trường hợp ranh giới tối thiểu | 
| Phần tử đơn quá lớn | 0 | độ chính xác của lựa chọn không | 
| Giá trị lớn hỗn hợp | 1 | hành vi tham lam dừng lại | 
| Tất cả các giá trị nhỏ | 5 | trường hợp sử dụng đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi chỉ tồn tại một tệp. Thuật toán vẫn hoạt động chính xác vì vòng lặp tăng một lần hoặc ngắt ngay lập tức, tạo ra 1 hoặc 0 tùy thuộc vào dung lượng. 

Một trường hợp khác là khi tất cả các tệp vượt quá X. Sau khi sắp xếp, phần tử đầu tiên đã không đáp ứng điều kiện, do đó số lượng vẫn bằng 0 mà không có bất kỳ biến chứng lặp lại nào. 

Trường hợp thứ ba là khi tất cả các tệp đều rất nhỏ so với X. Vòng lặp chỉ tiêu thụ tất cả các phần tử và tổng không bao giờ vượt quá X, do đó câu trả lời bằng N. 

Mỗi trường hợp này được xử lý thống nhất theo cùng một điều kiện tham lam và tính chính xác không phụ thuộc vào logic phân nhánh hoặc vỏ đặc biệt ngoài quá trình quét được sắp xếp.
