---
title: "CF 102881K - Tưới cây"
description: "Vấn đề mô tả một hàng cây. Cây i bắt đầu với chiều cao hi và tăng lên theo đơn vị gi sau mỗi đơn vị thời gian. Chúng ta cần tìm số nguyên đầu tiên t khi các cây được xếp từ trái sang phải theo thứ tự chiều cao không giảm dần."
date: "2026-07-25T12:37:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "K"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 38
verified: true
draft: false
---

[CF 102881K - Tưới cây](https://codeforces.com/problemset/problem/102881/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một hàng cây. Thực vật`i`bắt đầu với chiều cao`h_i`và phát triển bởi`g_i`đơn vị sau mỗi đơn vị thời gian. Chúng ta cần tìm số nguyên đầu tiên`t`khi các cây từ trái sang phải được xếp theo thứ tự chiều cao không giảm dần. Nếu đơn hàng không bao giờ được sắp xếp, chúng tôi phải báo cáo`-1`. 

Dữ liệu đầu vào cung cấp số lượng cây, chiều cao ban đầu và tốc độ tăng trưởng. Đầu ra là khoảnh khắc số nguyên không âm nhỏ nhất khi mọi cặp lân cận đều thỏa mãn`height[i] <= height[i + 1]`. 

Quan sát quan trọng là điều kiện cuối cùng chỉ phụ thuộc vào các cây lân cận. Nếu mỗi cây không cao hơn cây tiếp theo thì toàn bộ chuỗi sẽ được sắp xếp. Đối với mỗi cặp liền kề, chúng ta chỉ cần biết khi nào bất đẳng thức trở thành đúng. 

Ràng buộc`n <= 100000`có nghĩa là một cách tiếp cận kiểm tra nhiều lần có thể là không thể. Câu trả lời có thể rất lớn vì chiều cao và tốc độ tăng trưởng có thể đạt tới`10^9`, vì vậy việc tìm kiếm nhị phân đúng thời gian sẽ yêu cầu xử lý cẩn thận và không cần thiết. Chúng ta cần một giải pháp xử lý từng cặp liền kề một lần, đưa ra`O(n)`thuật toán. 

Một sai lầm phổ biến là mô phỏng tăng trưởng từng bước. Ví dụ: nếu một cây phát triển nhanh hơn nhiều so với cây khác, thời gian giao thoa có thể lên tới hàng tỷ hoặc hơn, khiến việc mô phỏng trở nên vô vọng. Một sai lầm khác là chỉ kiểm tra những cây cao nhất và thấp nhất. Sắp xếp là một thuộc tính cục bộ, vì vậy mọi cặp lân cận đều quan trọng. 

Hãy xem xét đầu vào:```
3
5 4 3
1 2 3
```Đầu ra đúng là`1`. Vào thời điểm`0`độ cao là`5, 4, 3`, không được sắp xếp. Sau một lúc họ trở thành`6, 6, 6`. Việc triển khai bất cẩn chỉ kiểm tra xem giá trị lớn nhất cuối cùng có ngừng lớn nhất hay không có thể bỏ lỡ thực tế là mối quan hệ ở giữa cũng được yêu cầu. 

Một trường hợp khác là khi hai cây có tốc độ tăng trưởng giống hệt nhau nhưng sai thứ tự.```
2
10 5
3 3
```Đầu ra đúng là`-1`. Sự khác biệt của chúng không bao giờ thay đổi, vì vậy cây đầu tiên sẽ luôn cao hơn. Việc triển khai chia cho chênh lệch tăng trưởng mà không xử lý số 0 sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là mô phỏng thời gian. Tại mỗi thời điểm, chúng tôi cập nhật tất cả chiều cao của cây và kiểm tra xem trình tự đã được sắp xếp chưa. Điều này đúng vì chúng ta thực sự đang tuân theo quá trình được mô tả bởi bài toán. Tuy nhiên, thời gian hợp lệ đầu tiên có thể cực kỳ lớn. Nếu câu trả lời là xung quanh`10^9`, mô phỏng sẽ yêu cầu khoảng`10^14`hoạt động bởi vì mỗi bước kiểm tra tất cả`n`thực vật. 

Quan sát hữu ích là mọi cặp lân cận đều đưa ra một hạn chế toán học về thời gian. Đối với cây trồng`i`Và`i + 1`, chúng ta cần:`h_i + g_i * t <= h_(i+1) + g_(i+1) * t`Sắp xếp lại mang lại:`(g_i - g_(i+1)) * t <= h_(i+1) - h_i`Đây là bất đẳng thức duy nhất về`t`. Nếu cây bên trái phát triển nhanh hơn thì sự bất bình đẳng sẽ tạo ra giới hạn trên về thời gian. Nếu cây phù hợp phát triển nhanh hơn, nó sẽ tạo ra giới hạn thấp hơn. Nếu cả hai tốc độ tăng trưởng bằng nhau thì thứ tự ban đầu không bao giờ có thể thay đổi. 

Sau khi thu thập tất cả các giới hạn dưới và giới hạn trên, câu trả lời đơn giản là số nguyên không âm nhỏ nhất thỏa mãn chúng. Lực lượng vũ phu hoạt động vì nó tìm kiếm trực tiếp dòng thời gian, nhưng cách tiếp cận bất bình đẳng hoạt động vì mỗi cặp mô tả toàn bộ mối quan hệ trong tương lai cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời * n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với nhiều câu trả lời có thể có. Thời gian hợp lệ nhỏ nhất là`0`, vì vậy hãy khởi tạo giới hạn dưới thành`0`. Giới hạn trên là không giới hạn. 
2. Đối với mỗi cặp cây liền kề, hãy so sánh chiều cao tương lai của chúng. Điều kiện cần duy trì là:`h_i + g_i*t <= h_(i+1) + g_(i+1)*t`Sắp xếp lại bất đẳng thức này cho chúng ta biết cặp này giới hạn thời gian sớm nhất có thể hay thời gian muộn nhất có thể. 
3. Nếu`g_i - g_(i+1)`dương thì cây bên trái tăng chiều cao nhanh hơn. Cặp này chỉ có thể được sắp xếp trước một khoảng thời gian tối đa nào đó, vì vậy chúng tôi cập nhật giới hạn trên. 
4. Nếu`g_i - g_(i+1)`âm thì cây bên phải tăng chiều cao nhanh hơn. Cặp này cuối cùng trở nên chính xác, nhưng chỉ sau một khoảng thời gian tối thiểu, vì vậy chúng tôi cập nhật giới hạn dưới. 
5. Nếu`g_i - g_(i+1)`bằng 0 thì cả hai cây luôn thay đổi một lượng như nhau. Thứ tự của chúng không bao giờ thay đổi, vì vậy sự so sánh ban đầu sẽ quyết định liệu câu trả lời có khả thi hay không. 
6. Sau khi tất cả các cặp được xử lý, nếu giới hạn dưới lớn hơn giới hạn trên thì không có thời gian nào thỏa mãn mọi điều kiện. Ngược lại, giới hạn dưới là khoảnh khắc số nguyên hợp lệ đầu tiên. 

Tại sao nó hoạt động: mọi chuỗi được sắp xếp phải thỏa mãn mọi bất đẳng thức liền kề. Thuật toán chuyển đổi từng bất đẳng thức đó thành một hạn chế về các giá trị thời gian có thể có. Giao điểm của tất cả các hạn chế chính xác là tập hợp thời gian khi toàn bộ chuỗi được sắp xếp. Lấy giá trị không âm nhỏ nhất trong giao điểm đó sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))
    g = list(map(int, input().split()))

    low = 0
    high = 10**30

    for i in range(n - 1):
        dg = g[i] - g[i + 1]
        dh = h[i + 1] - h[i]

        if dg == 0:
            if dh < 0:
                print(-1)
                return
        elif dg > 0:
            high = min(high, dh // dg)
        else:
            need = (-dh + (-dg) - 1) // (-dg)
            low = max(low, need)

    if low <= high:
        print(low)
    else:
        print(-1)

if __name__ == "__main__":
    solve()
```Mã giữ hai biến đại diện cho giao điểm của tất cả các thời điểm hợp lệ.`low`lưu trữ câu trả lời sớm nhất có thể, trong khi`high`lưu trữ câu trả lời mới nhất có thể. 

Để có chênh lệch tăng trưởng dương, phép chia số nguyên là an toàn vì bất đẳng thức yêu cầu`t <= dh / dg`. Nếu như`dh`là số âm, kết quả là giới hạn trên trở thành số âm, điều này tự động làm cho phạm vi cuối cùng không hợp lệ vì thời gian không thể âm. 

Đối với chênh lệch tăng trưởng âm, mã sẽ tính mức trần của việc phân chia. Điều này là bắt buộc vì thời gian phải là số nguyên. Việc sử dụng phép chia số nguyên thông thường ở đây sẽ làm tròn sai hướng đối với các giá trị âm và có thể tạo ra kết quả quá nhỏ một đơn vị. 

Các giá trị có thể đạt tới`10^9`và câu trả lời có thể vượt quá giới hạn số nguyên 32 bit, do đó, số nguyên chính xác tùy ý của Python sẽ tránh được sự cố tràn một cách tự nhiên. 

## Ví dụ đã hoạt động 

Mẫu 1:```
5
1 2 3 4 5
5 4 3 2 1
```| Cặp | Sự khác biệt tăng trưởng | Chênh lệch chiều cao | Hạn chế | Phạm vi hiện tại | 
| --- | --- | --- | --- | --- | 
| 1,2 | 1 | 1 | t <= 1 | 0 <= t <= 1 | 
| 2,3 | 1 | 1 | t <= 1 | 0 <= t <= 1 | 
| 3,4 | 1 | 1 | t <= 1 | 0 <= t <= 1 | 
| 4,5 | 1 | 1 | t <= 1 | 0 <= t <= 1 | 

Thời gian sớm nhất có thể là`0`và trình tự đã được sắp xếp nên câu trả lời là`0`. 

Mẫu 2:```
4
2 3 1 4
1 2 4 3
```| Cặp | Sự khác biệt tăng trưởng | Chênh lệch chiều cao | Hạn chế | Phạm vi hiện tại | 
| --- | --- | --- | --- | --- | 
| 1,2 | -1 | 1 | t >= 1 | 1 <= t | 
| 2,3 | -2 | -2 | t >= 1 | 1 <= t | 
| 3,4 | 1 | 3 | t <= 3 | 1 <= t <= 3 | 

Thời gian nguyên hợp lệ đầu tiên là`1`. Lúc đó độ cao trở nên`3, 5, 5, 7`, được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cặp lân cận được xử lý một lần. | 
| Không gian | O(1) | Chỉ giới hạn hiện tại được lưu trữ sau khi đọc mảng. | 

Thuật toán dễ dàng phù hợp với`100000`giới hạn của cây vì nó thực hiện một lượng công việc không đổi cho mỗi cặp liền kề. Nó tránh mọi sự phụ thuộc vào tầm quan trọng của câu trả lời. 

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

assert run("""5
1 2 3 4 5
5 4 3 2 1
""") == "0\n", "sample 1"

assert run("""4
2 3 1 4
1 2 4 3
""") == "1\n", "sample 2"

assert run("""2
10 5
3 3
""") == "-1\n", "equal growth cannot fix order"

assert run("""1
7
100
""") == "0\n", "single plant is always sorted"

assert run("""3
5 4 3
1 2 3
""") == "1\n", "multiple plants meet at the same time"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tăng chiều cao khi tốc độ tăng trưởng giảm |`0`| Trình tự đã được sắp xếp | 
| Đặt hàng hỗn hợp với tốc độ tăng trưởng khác nhau |`1`| Giới hạn dưới và trên giao nhau | 
| Tốc độ tăng trưởng ngang bằng với đơn hàng xấu |`-1`| Xử lý trường hợp bất khả thi | 
| Một cây |`0`| Ranh giới kích thước tối thiểu | 
| Cây lai đồng loạt |`1`| Tính toán trần số nguyên chính xác | 

## Vỏ cạnh 

Để có tốc độ tăng trưởng bằng nhau, thuật toán không thực hiện phép chia. Ví dụ:```
2
10 5
3 3
```Sự chênh lệch tăng trưởng bằng 0 và cây đầu tiên bắt đầu cao hơn. Vì cả hai cây đều tăng chiều cao với tốc độ như nhau nên sự khác biệt về`5`không bao giờ thay đổi. Thuật toán ngay lập tức trở lại`-1`. 

Đối với một nhà máy duy nhất:```
1
100
50
```Không có cặp liền kề nào vi phạm điều kiện đặt hàng. Giới hạn dưới ban đầu vẫn còn`0`, vậy câu trả lời là`0`. 

Đối với trường hợp một cặp được sắp xếp chính xác tại một thời điểm nhỏ, mức trần số nguyên có ý nghĩa quan trọng:```
2
10 0
1 3
```Bất đẳng thức trở thành`10 + t <= 3t`, Vì thế`10 <= 2t`, nghĩa`t >= 5`. Thuật toán sử dụng phép chia trần và trả về`5`, đây là khoảnh khắc số nguyên đầu tiên khi điều kiện đúng. 

Đối với các hạn chế xung đột, thuật toán sẽ phát hiện một phạm vi trống. Ví dụ: một cặp có thể yêu cầu`t >= 5`trong khi cái khác yêu cầu`t <= 3`. Vì không có số nguyên nào có thể thỏa mãn cả hai nên kiểm tra cuối cùng`low <= high`thất bại và câu trả lời là`-1`.
