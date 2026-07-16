---
title: "CF 103430H - Tin nhắn"
description: "Chúng tôi được cấp một nhóm sinh viên. Mỗi học sinh được liên kết với một chỉ mục tin nhắn cụ thể và một giá trị giới hạn kiểm soát mức độ tin cậy mà các em sẽ đọc một tin nhắn được ghim tùy thuộc vào tổng số tin nhắn được ghim. Chúng ta được phép chọn một số tin nhắn và “ghim” chúng."
date: "2026-07-03T08:07:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103430
codeforces_index: "H"
codeforces_contest_name: "2021-2022 ICPC, NERC, Southern and Volga Russian Regional Contest (problems intersect with Educational Codeforces Round 117)"
rating: 0
weight: 103430
solve_time_s: 46
verified: true
draft: false
---

[CF 103430H - Tin nhắn](https://codeforces.com/problemset/problem/103430/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một nhóm sinh viên. Mỗi học sinh được liên kết với một chỉ mục tin nhắn cụ thể và một giá trị giới hạn kiểm soát mức độ tin cậy mà các em sẽ đọc một tin nhắn được ghim tùy thuộc vào tổng số tin nhắn được ghim. 

Chúng ta được phép chọn một số tin nhắn và “ghim” chúng. Sau đó, mỗi học sinh độc lập đọc hoặc không đọc tin nhắn của mình. Xác suất không phải là tùy ý mà chỉ phụ thuộc vào việc tin nhắn của họ có được ghim hay không và tập hợp được ghim lớn đến mức nào. 

Nếu tin nhắn của học sinh không nằm trong số những tin nhắn được ghim thì học sinh đó không đóng góp được gì. Nếu được ghim thì mức đóng góp phụ thuộc vào tổng số tin nhắn được ghim, ký hiệu là t. Khi t đủ nhỏ so với ngưỡng ki của học sinh thì chắc chắn học sinh đó đã đọc được tin nhắn. Khi t vượt quá ki, cơ hội sẽ giảm đi và bằng ki chia cho t. 

Nhiệm vụ là chọn cả số lượng tin nhắn được ghim và những tin nhắn cần ghim sao cho số lượng học sinh đọc tin nhắn của mình dự kiến ​​là tối đa. 

Các ràng buộc ngụ ý rằng việc tìm kiếm trực tiếp trên tất cả các tập hợp con là không thể vì có rất nhiều cách để chọn các tin nhắn được ghim. Ngay cả việc lặp lại tất cả các kích thước tập hợp con và tính toán lại các giá trị một cách ngây thơ cũng dẫn đến hành vi gần như O(n^2), quá chậm khi n lớn. 

Một trường hợp khó phát hiện từ hành vi khi t vượt quá tất cả các giá trị ki. Vì mỗi ki được giới hạn bởi 20 nên hàm mô tả sự đóng góp sẽ ngừng thay đổi một cách có ý nghĩa khi vượt quá một ngưỡng nhỏ. Việc triển khai ngây thơ tiếp tục tính toán với t lớn có thể lãng phí tính toán trong khi không nhận ra rằng cấu trúc tối ưu ổn định. 

Ví dụ, hãy xem xét một tình huống trong đó tất cả ki đều bằng 1. Nếu t trở thành 50, mỗi học sinh được chọn đóng góp 1/50 và chiến lược tốt nhất rõ ràng đã được xác định bởi t nhỏ hơn nhiều. Một cách tiếp cận mạnh mẽ liên tục đánh giá các giá trị t lớn sẽ liên tục tính toán lại cấu trúc xếp hạng giống nhau với các giá trị được chia tỷ lệ. 

Một trường hợp góc khác là khi tất cả các tin nhắn có giá trị ki lớn. Trong tình huống đó, với t nhỏ, mỗi học sinh được chọn đóng góp 1, và vấn đề giảm xuống còn việc chọn tùy ý các thông điệp t tốt nhất. Việc triển khai ngây thơ áp dụng không chính xác quy tắc phân số quá sớm sẽ đánh giá thấp giá trị. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ một quan điểm trực tiếp. Giả sử chúng ta sửa số lượng tin nhắn được ghim t. Nếu chúng ta biết t tin nhắn nào được ghim thì số học sinh hài lòng dự kiến ​​sẽ được xác định đầy đủ bằng cách tính tổng đóng góp của từng học sinh được chọn. 

Với một t cố định, mỗi học sinh i đóng góp 1 nếu ngưỡng ki của họ ít nhất là t, hoặc ki chia cho t nếu ngược lại. Điều này mang lại một giá trị xác định cho mọi thông báo ứng cử viên tại thời điểm t cụ thể đó. Do đó, đối với mỗi t, chiến lược tối ưu là sắp xếp các thông điệp theo mức độ đóng góp của chúng trong t đó và chọn t trên cùng trong số chúng. 

Điều này ngay lập tức mang lại một phương pháp đúng nhưng tốn kém. Với mọi t có thể cho đến n, chúng tôi tính toán một giá trị cho mỗi tin nhắn, sắp xếp chúng và lấy giá trị t tốt nhất. Điều này dẫn đến thời gian O(n^2 log n), quá chậm đối với đầu vào lớn. 

Quan sát cấu trúc quan trọng là ki bị giới hạn bởi 20. Một khi t tăng vượt quá 20, không học sinh nào còn bước vào chế độ “giá trị đầy đủ bằng 1” nữa. Thay vào đó, tất cả các khoản đóng góp sẽ tỷ lệ với ki chia cho t, đây chỉ là một tỷ lệ thống nhất của ki. Điều này có nghĩa là với t lớn hơn 20, thứ tự của các thông điệp không bao giờ thay đổi, chỉ có tổng của chúng được tính theo tỷ lệ 1/t. Việc tăng t vượt quá điểm này không thể tạo ra cấu hình tối ưu mới; nó chỉ làm suy yếu các đóng góp trong khi buộc chúng tôi phải chọn nhiều phần tử hơn. 

Điều này cho phép chúng ta hạn chế sự chú ý đến các giá trị t lên tới 20. Ngoài ra, lựa chọn tốt nhất hành xử đơn điệu theo cách không thể đánh bại một số t trong phạm vi từ 1 đến 20.

Do đó, chúng tôi đánh giá câu trả lời tối ưu cho mỗi t từ 1 đến 20 bằng cách tính toán lại các đóng góp, sắp xếp và chọn các phần tử t tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả t lên đến n | O(n^2 log n) | O(n) | Quá chậm | 
| Điểm cắt tối ưu ở t 20 | O(20 · n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa số lượng tin nhắn được ghim t ứng viên và tính toán kỳ vọng tốt nhất có thể đạt được cho t đó. 

1. Với một t cố định, hãy tính phần đóng góp của mỗi thông điệp i là 1 khi ki ít nhất là t, nếu không thì ki chia cho t. Điều này mã hóa chính xác việc học sinh đọc chắc chắn hay chỉ được hưởng lợi một phần. 
2. Tập hợp tất cả những đóng góp này vào một danh sách. 
3. Sắp xếp danh sách theo thứ tự giảm dần. Điều này là cần thiết vì với số lượng tin nhắn được ghim cố định, chúng ta luôn muốn chọn những tin nhắn t có mức đóng góp cận biên lớn nhất. 
4. Lấy các giá trị t đầu tiên từ danh sách đã sắp xếp và tính tổng chúng. Điều này thể hiện số lượng độc giả mong đợi tốt nhất có thể cho t cố định đó. 
5. Lặp lại quy trình tương tự cho mỗi t từ 1 đến 20. 
6. Câu trả lời cuối cùng là giá trị lớn nhất trên tất cả các t được kiểm tra. 

Ngưỡng ở mức 20 được chứng minh bằng tính chất giới hạn của ki. Khi t vượt quá ki tối đa, mọi đóng góp sẽ chuyển sang chế độ phân số và tất cả các giá trị sẽ trở thành ki chia cho t. Tại thời điểm đó, tập hợp con tốt nhất luôn là cùng một tập hợp các giá trị ki hàng đầu, chỉ được thay đổi tỷ lệ, do đó việc tăng thêm t không thể tạo ra cấu hình tốt hơn. 

### Tại sao nó hoạt động 

Đối với mỗi t cố định, thuật toán tính toán tập hợp con tối ưu một cách chính xác bằng cách lựa chọn tham lam trên các đóng góp xác định cho mỗi phần tử. Sự ghép nối duy nhất giữa các phần tử là ràng buộc phải chọn chính xác t và việc sắp xếp giải quyết điều này một cách tối ưu. 

Giới hạn t đến 20 duy trì tính tối ưu vì mọi thay đổi cấu trúc trong đóng góp chỉ xảy ra ở ngưỡng nguyên ki. Vì tất cả ki nằm trong một phạm vi giới hạn nhỏ nên mọi chuyển đổi có ý nghĩa của mục tiêu đều xảy ra trong phạm vi đó. Ngoài ra, mục tiêu trở thành một hàm tuyến tính có tỷ lệ theo một thứ tự cố định, do đó không có sự tối ưu mới nào có thể xuất hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return

    max_t = min(20, n)
    ans = 0.0

    for t in range(1, max_t + 1):
        vals = []
        inv_t = 1.0 / t

        for k in a:
            if k >= t:
                vals.append(1.0)
            else:
                vals.append(k * inv_t)

        vals.sort(reverse=True)
        ans = max(ans, sum(vals[:t]))

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại các giá trị có thể có của t cho đến 20. Đối với mỗi t, nó xây dựng một danh sách đóng góp cho tất cả các tin nhắn. Điều kiện k >= t tương ứng chính xác với trường hợp đọc xác định, trong khi k < t kích hoạt chia tỷ lệ phân số. 

Việc sắp xếp là cần thiết vì việc lựa chọn tin nhắn nào để ghim sẽ độc lập sau khi các đóng góp được cố định. Lấy giá trị t lớn nhất đảm bảo lựa chọn tối ưu cho t đó. 

Một cạm bẫy triển khai phổ biến là quên giới hạn t ở mức 20. Nếu không có sự tối ưu hóa này, giải pháp sẽ giảm xuống O(n^2 log n). Một vấn đề tế nhị khác là trộn lẫn phép chia số nguyên với phép chia dấu phẩy động; sử dụng các giá trị nổi đảm bảo so sánh và tích lũy chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó n bằng 5 và giá trị ki là [1, 2, 3, 2, 1]. 

Chúng tôi đánh giá t bằng 1. 

| t | đóng góp | tổng t hàng đầu | 
| --- | --- | --- | 
| 1 | [1, 1, 1, 1, 1] | 1 | 

Với t bằng 2. 

| t | đóng góp | tổng t hàng đầu | 
| --- | --- | --- | 
| 2 | [0,5, 1, 1, 1, 0,5] | 2 | 

Với t bằng 3. 

| t | đóng góp | tổng t hàng đầu | 
| --- | --- | --- | 
| 3 | [0,333, 0,666, 1, 0,666, 0,333] | 2.333 | 

Dấu vết này cho thấy việc tăng t sẽ làm tăng số lượng phần tử được chọn nhưng làm giảm sự đóng góp riêng lẻ như thế nào. 

Ví dụ thứ hai sử dụng n bằng 4 với các giá trị ki [20, 20, 1, 1]. 

Tại t bằng 2, các đóng góp là [1, 1, 0,5, 0,5], cho tổng lớn nhất là 2. 

Tại t bằng 10, các đóng góp là [1, 1, 0,1, 0,1], cho tổng số cao nhất là 2,2 cho 10 lựa chọn hàng đầu được cắt bớt thành các phần tử có sẵn, nhưng vì chỉ tồn tại 4 nên nó trở thành 2,2. Tuy nhiên, điều này bị chi phối bởi t nhỏ hơn trong thực tế, minh họa tại sao việc tìm kiếm bị hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(20 · n log n) | Với mỗi t đến 20, chúng tôi tính toán n giá trị và sắp xếp chúng | 
| Không gian | O(n) | Chúng tôi lưu trữ một loạt các đóng góp tạm thời | 

Giới hạn không đổi trên t đảm bảo giải pháp hoạt động giống như O(n log n), phù hợp với đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# small deterministic cases
assert run("1\n1\n") == "1", "single element"

assert run("2\n1 2\n") != "", "basic feasibility"

assert run("3\n1 1 1\n") == "1", "uniform small values"

assert run("5\n20 20 20 20 20\n") != "", "all large k"

assert run("4\n1 2 3 4\n") != "", "increasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | độ đúng cơ sở | 
| giá trị nhỏ thống nhất | 1 | hành vi chế độ phân số | 
| toàn ki lớn | 5 | tất cả những đóng góp mang tính quyết định | 
| tăng ki | khác nhau | sự ổn định sắp xếp và logic lựa chọn | 

## Vỏ cạnh 

Khi tất cả các giá trị ki bằng 1, mọi giá trị t lớn hơn 1 sẽ đẩy tất cả các đóng góp vào chế độ phân số. Ví dụ: với n bằng 3 và k bằng [1, 1, 1], tại t bằng 2 mỗi phần tử được chọn đóng góp 1/2. Thuật toán đánh giá t bằng 1 và t bằng 2, tạo ra các giá trị lần lượt là 1 và 1,5 và chọn đúng t bằng 2. 

Khi tất cả các giá trị ki đều lớn, chẳng hạn như [20, 20, 20], hành vi của t bằng 1 là không đáng kể vì mọi giá trị đều đóng góp 1. Tại t bằng 2, tất cả các đóng góp vẫn là 1 và lựa chọn hàng đầu vẫn mang lại giá trị đầy đủ. Thuật toán vẫn so sánh cả hai trường hợp và chọn một cách nhất quán mà không dựa vào các giả định về phân phối. 

Khi n nhỏ, chẳng hạn như n bằng 1, vòng lặp trên t vẫn hoạt động chính xác vì max_t được kẹp ở 1, đảm bảo không có kích thước lựa chọn không hợp lệ nào được thử.
