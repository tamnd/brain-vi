---
title: "CF 105093E - Biểu đồ con kéo dài chi phí tối thiểu"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có $n$ quốc gia và mỗi quốc gia có một giá trị nguyên duy nhất biểu thị trình độ công nghệ của quốc gia đó."
date: "2026-06-27T20:50:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "E"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 49
verified: true
draft: false
---

[CF 105093E - Biểu đồ con mở rộng chi phí tối thiểu](https://codeforces.com/problemset/problem/105093/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$quốc gia và mỗi quốc gia có một giá trị số nguyên duy nhất thể hiện trình độ công nghệ của quốc gia đó. 

Hai quốc gia có thể được kết nối trực tiếp bằng cáp truyền thông khi và chỉ khi sự khác biệt tuyệt đối về trình độ công nghệ của họ không vượt quá ngưỡng đã chọn$a$. Nếu có một sợi cáp như vậy thì nó luôn được xây dựng. Có thể liên lạc giữa hai quốc gia nếu tồn tại một chuỗi cáp tạo thành đường dẫn giữa chúng. 

Mục đích là chọn giá trị nhỏ nhất có thể của$a$sao cho sau khi xây dựng xong tất cả các loại cáp được phép, mọi quốc gia đều có thể tiếp cận mọi quốc gia khác thông qua một số chuỗi kết nối. 

Kích thước đầu vào gợi ý lên tới$10^5$tổng giá trị trên tất cả các trường hợp thử nghiệm, loại trừ mọi phương pháp bậc hai so sánh trực tiếp tất cả các cặp. Bất kỳ giải pháp nào kiểm tra tất cả các cặp quốc gia sẽ yêu cầu tới$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Do đó, chúng ta cần một cấu trúc tránh suy luận rõ ràng trên tất cả các cạnh. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi các giá trị không được sắp xếp. Ví dụ: nếu các giá trị là: 

đầu vào:```
1
4
1 100 2 3
```Nếu một người lý luận không chính xác về các kết nối cặp tùy ý mà không có thứ tự, thì rất dễ bỏ sót rằng “khoảng cách” ngăn cản kết nối thực sự nằm trong khoảng từ 3 đến 100, chứ không phải giữa các phần tử ở xa tùy ý. Câu trả lời đúng chỉ phụ thuộc vào nút cổ chai chặt chẽ nhất trong thứ tự chứ không phải khoảng cách cặp tùy ý. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau, chẳng hạn như:```
1
5
7 7 7 7 7
```Ở đây bất kỳ$a = 0$đã kết nối mọi thứ, vì mọi sự khác biệt đều bằng không. 

Những quan sát này gợi ý rằng cấu trúc của kết nối tối ưu phụ thuộc vào cách các giá trị sắp xếp theo thứ tự được sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là coi mỗi cặp quốc gia là một cạnh tiềm năng và mô phỏng những cạnh nào tồn tại với một giá trị cố định là$a$. Đối với một nhất định$a$, chúng ta có thể xây dựng biểu đồ và chạy BFS hoặc DSU để kiểm tra kết nối. Sau đó chúng ta có thể tìm kiếm nhị phân$a$, kiểm tra kết nối mỗi lần. 

Điều này hoạt động chính xác vì kết nối đơn điệu trong$a$, nhưng mỗi lần kiểm tra đều yêu cầu kiểm tra tất cả$O(n^2)$cặp trong trường hợp xấu nhất. Ngay cả khi được tối ưu hóa, việc xây dựng các cạnh vẫn tốn thời gian bậc hai, điều này không thành công theo$n = 10^5$. 

Quan sát quan trọng là sau khi sắp xếp các giá trị công nghệ, các cạnh duy nhất quan trọng đối với khả năng kết nối là giữa các phần tử liền kề theo thứ tự được sắp xếp. Nếu có một khoảng cách lớn giữa hai giá trị được sắp xếp liên tiếp thì không có giá trị nào nhỏ hơn ở giữa tồn tại để “thu hẹp” khoảng cách đó. Bất kỳ con đường nào cố gắng vượt qua nó vẫn phải vượt qua bước nhảy số đó, điều này là không thể trừ khi$a$ít nhất là khoảng cách đó. 

Do đó, toàn bộ vấn đề rút gọn thành việc tìm sự khác biệt lớn nhất giữa các phần tử liên tiếp trong mảng đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ trên$a$) |$O(n^2)$mỗi lần kiểm tra |$O(n^2)$| Quá chậm | 
| Tối ưu (sắp xếp + quét) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng giá trị công nghệ theo thứ tự không giảm. Việc sắp xếp cho thấy cấu trúc tự nhiên của những “bước nhảy” tối thiểu giữa các giá trị, đây là nơi kết nối có thể bị lỗi. 
2. Tính hiệu giữa từng cặp phần tử liên tiếp trong mảng đã được sắp xếp. Mỗi sự khác biệt thể hiện ngưỡng yêu cầu nhỏ nhất để trực tiếp thu hẹp khoảng cách cục bộ đó. 
3. Theo dõi mức tối đa của những khác biệt liên tiếp này. Khoảng cách tối đa này là mắt xích yếu nhất trong chuỗi giá trị. 
4. Đưa ra khoảng cách tối đa này làm câu trả lời cho trường hợp thử nghiệm, vì bất kỳ giá trị nhỏ hơn nào cũng sẽ để lại ít nhất một sự gián đoạn không thể tránh khỏi trong kết nối. 

### Tại sao nó hoạt động 

Sau khi các giá trị được sắp xếp, mọi đường đi từ phần tử nhỏ nhất đến phần tử lớn nhất đều phải di chuyển qua các giá trị trung gian theo thứ tự tăng dần. Ngay cả khi các cạnh tồn tại giữa các nút không liền kề, bất kỳ kết nối nào như vậy vẫn không thể vượt qua khoảng cách lớn nhất giữa các giá trị được sắp xếp liên tiếp, vì không có giá trị trung gian nào tồn tại để giảm bước nhảy đó. Biểu đồ trở nên được kết nối chính xác khi mọi khoảng trống liền kề đều “có thể bắc cầu” và khoảng cách tồi tệ nhất chỉ ra ngưỡng yêu cầu tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        arr = list(map(int, input().split()))
        arr.sort()
        
        ans = 0
        for i in range(1, n):
            ans = max(ans, arr[i] - arr[i - 1])
        
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Sắp xếp là bước quan trọng vì nó biến một vấn đề kết nối dày đặc thành một quá trình quét tuyến tính đối với những khác biệt liền kề. Câu trả lời được tích lũy bằng cách theo dõi khoảng cách tối đa, tương ứng trực tiếp với ngưỡng tối thiểu cần thiết để kết nối đầy đủ. 

Một sai lầm phổ biến là cố gắng giải thích tất cả những khác biệt theo cặp. Điều đó vượt quá các ràng buộc không liên quan. Chỉ những khác biệt liên tiếp trong thứ tự sắp xếp mới quan trọng vì chúng đại diện cho các dấu phân cách tối thiểu trong dòng giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
1 100 2 3
```Mảng được sắp xếp:$[1, 2, 3, 100]$| Bước | Mảng được sắp xếp | Khoảng cách hiện tại | Khoảng cách tối đa | 
| --- | --- | --- | --- | 
| 1 | [1, 2, 3, 100] | 1 - | 1 | 
| 2 | [1, 2, 3, 100] | 1 | 1 | 
| 3 | [1, 2, 3, 100] | 1 | 1 | 
| 4 | [1, 2, 3, 100] | 97 | 97 | 

Câu trả lời cuối cùng là 97. 

Dấu vết này cho thấy toàn bộ cấu trúc bị hạn chế bởi bước nhảy từ 3 lên 100. Không có nút trung gian nào tồn tại để giảm yêu cầu đó. 

### Ví dụ 2 

đầu vào:```
1
5
5 5 5 5 5
```Mảng được sắp xếp:$[5, 5, 5, 5, 5]$| Bước | Mảng được sắp xếp | Khoảng cách hiện tại | Khoảng cách tối đa | 
| --- | --- | --- | --- | 
| 1 | [5, 5, 5, 5, 5] | 0 | 0 | 
| 2 | [5, 5, 5, 5, 5] | 0 | 0 | 
| 3 | [5, 5, 5, 5, 5] | 0 | 0 | 
| 4 | [5, 5, 5, 5, 5] | 0 | 0 | 

Câu trả lời cuối cùng là 0. 

Điều này xác nhận rằng các giá trị giống hệt nhau không yêu cầu ngưỡng nào cả vì kết nối đã hoàn tất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tuyến tính sau đó | 
| Không gian |$O(n)$| Lưu trữ cho mảng | 

Tổng cộng$N$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, do đó, việc sắp xếp từng trường hợp thử nghiệm một cách độc lập vẫn phù hợp thoải mái trong giới hạn và việc quét tuyến tính sẽ bổ sung thêm chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(data: str) -> str:
    sys.stdin = io.StringIO(data)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-style cases
assert solve_io("""1
4
1 100 2 3
""") == "97"

assert solve_io("""1
5
5 5 5 5 5
""") == "0"

# minimum size
assert solve_io("""1
2
1 10
""") == "9"

# already sorted small gaps
assert solve_io("""1
3
1 2 10
""") == "8"

# multiple test cases
assert solve_io("""2
3
1 3 6
4
10 1 2 3
""") == "3\n9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng cách lớn duy nhất | 97 | phát hiện sự phân tách trội | 
| tất cả đều bình đẳng | 0 | xử lý câu trả lời bằng không | 
| hai yếu tố | sự khác biệt trực tiếp | trường hợp cơ sở đúng đắn | 
| thứ tự hỗn hợp | 8 | sắp xếp đúng đắn | 
| nhiều bài kiểm tra | đầu ra hỗn hợp | trộn chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$n = 2$. Thuật toán giảm xuống thành phép trừ trực tiếp sau khi sắp xếp và chênh lệch liên tiếp tối đa chỉ đơn giản là chênh lệch tuyệt đối của hai giá trị. Đối với đầu vào:```
1
2
8 3
```sắp xếp mang lại$[3, 8]$và khoảng cách duy nhất là 5, được trả về chính xác. 

Một trường hợp khác là các giá trị lặp lại được trộn lẫn với một ngoại lệ duy nhất. Vì:```
1
5
4 4 4 4 100
```phân loại sản lượng$[4, 4, 4, 4, 100]$. Thuật toán quét các điểm khác biệt liên tiếp và tìm thấy một khoảng cách lớn duy nhất là 96. Điều này nắm bắt chính xác rằng ngoại lệ bị ngắt kết nối khỏi cụm trừ khi ngưỡng ít nhất là 96.
