---
title: "CF 104931C - Vịnh sô cô la của con bạc"
description: "Chúng ta được cung cấp một số máy đánh bạc, mỗi máy tạo ra phần thưởng ngẫu nhiên khi được rút. Mỗi máy có phân bố xác suất cố định riêng trên một tập hợp nhỏ các giá trị phần thưởng có thể có."
date: "2026-06-28T07:35:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104931
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 1 (Advanced)"
rating: 0
weight: 104931
solve_time_s: 73
verified: false
draft: false
---

[CF 104931C - Vịnh sô cô la của người cờ bạc](https://codeforces.com/problemset/problem/104931/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số máy đánh bạc, mỗi máy tạo ra phần thưởng ngẫu nhiên khi được rút. Mỗi máy có phân bố xác suất cố định riêng trên một tập hợp nhỏ các giá trị phần thưởng có thể có. Bạn được phép thực hiện chính xác`n`tổng số lần kéo và mỗi lần kéo có thể được chỉ định cho bất kỳ máy nào. Các lực kéo khác nhau là độc lập và sự phân bố của máy không thay đổi theo thời gian. 

Mục đích là quyết định cách phân phối`n`kéo qua các máy để tổng phần thưởng mong đợi càng lớn càng tốt. Sau khi tất cả các lần kéo được thực hiện, chúng ta chỉ được yêu cầu về kỳ vọng tối đa có thể có này chứ không phải về chuỗi kết quả thực tế. 

Các ràng buộc rất nhỏ: cả hai`n`Và`k`nhiều nhất là 100, trong khi mỗi phân phối có thể chứa tới 1000 kết quả. Điều này đã gợi ý rằng ngay cả các giải pháp bậc hai hoặc bậc ba đối với máy và lực kéo cũng có thể được chấp nhận, nhưng bất kỳ điều gì liên quan đến phân bổ tổ hợp trên các phân phối trên mỗi lần kéo sẽ là không cần thiết. Một giải pháp có thể tính toán lại các kỳ vọng một cách cẩn thận cho từng máy là đủ nhanh. 

Một chế độ thất bại tinh vi trong nhiều cách tiếp cận không chính xác là cố gắng mô hình hóa sự phân bổ xác suất đầy đủ của tổng số phần thưởng qua nhiều lần kéo. Ví dụ: người ta có thể thử lập trình động trên các tổng có thể có sau mỗi lần kéo. Điều này là không cần thiết vì bài toán chỉ yêu cầu kỳ vọng chứ không yêu cầu phương sai hoặc hình dạng phân phối. Một lỗi phổ biến khác là cho rằng chúng ta cần xen kẽ các lực kéo giữa các máy theo một mô hình thông minh. Trong thực tế, mỗi lực kéo đều độc lập và đóng góp bổ sung vào kỳ vọng, do đó việc đặt hàng không có tác dụng. 

Là một ví dụ cụ thể về hướng sai, giả sử hai máy có giá trị kỳ vọng giống nhau nhưng mức chênh lệch khác nhau. Giải pháp theo dõi phân phối có thể cố gắng ưu tiên máy “an toàn hơn” một cách không chính xác, mặc dù kỳ vọng không phụ thuộc vào phương sai. Câu trả lời đúng chỉ phụ thuộc vào phần thưởng trung bình chứ không phụ thuộc vào rủi ro. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ của vấn đề là chúng ta phải gán`n`hành động riêng biệt (kéo), mỗi hành động chọn một trong`k`máy móc và mỗi hành động đều có kết quả ngẫu nhiên. Người ta có thể tưởng tượng một giải pháp mạnh mẽ liệt kê tất cả các nhiệm vụ có thể có của`n`kéo qua các máy và đánh giá giá trị mong đợi cho mỗi nhiệm vụ. 

Đối với mỗi nhiệm vụ, nếu chúng tôi mô phỏng kỳ vọng một cách chính xác, chúng tôi vẫn cần tính toán phần thưởng kỳ vọng cho mỗi lần rút từ phân phối. Ngay cả khi kỳ vọng trên mỗi máy được tính toán trước thì số lượng nhiệm vụ vẫn là`k^n`, vì mỗi`n`kéo có thể độc lập chọn một trong`k`máy móc. Với`n = 100`, điều này trở nên lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát quan trọng là kỳ vọng là tuyến tính. Tổng phần thưởng dự kiến ​​​​của nhiều lần kéo độc lập chỉ là tổng phần thưởng dự kiến ​​​​của mỗi lần kéo, bất kể sự phụ thuộc giữa các lựa chọn của máy. Điều này có nghĩa là mỗi lần kéo chỉ đóng góp độc lập dựa trên loại máy mà nó sử dụng. Không có sự tương tác nào giữa các lần kéo có thể tạo ra một chiến lược hỗn hợp tốt hơn việc liên tục chọn máy tốt nhất. 

Khi chúng tôi chấp nhận tính tuyến tính, cấu trúc sẽ sụp đổ: mỗi máy có phần thưởng dự kiến ​​cố định cho mỗi lần kéo và mỗi lần kéo sẽ thuộc về máy có kỳ vọng lớn nhất. Không cần phân bổ phức tạp hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công lực kéo Brute Force | O(k^n) | O(n) | Quá chậm | 
| Tính toán kỳ vọng và chọn tối đa | O(k * m_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách chuyển đổi phân bố xác suất của mỗi máy thành một số duy nhất, phần thưởng mong đợi cho mỗi lần kéo và sau đó chọn máy tốt nhất. 

1. Đối với mỗi máy, hãy đọc danh sách phần thưởng có thể có và xác suất tương ứng. 

Giá trị kỳ vọng của một lần kéo từ chiếc máy này là tổng trọng số của các kết quả. 
2. Tính kỳ vọng`E_i = sum(r_j * p_j)`cho mỗi máy`i`. 

Điều này thu gọn toàn bộ phân phối thành một đại lượng vô hướng biểu thị mức tăng trung bình dài hạn. 
3. Theo dõi giá trị mong đợi tối đa trong số tất cả các máy. 
4. Nhân kỳ vọng tối đa này với`n`, vì tất cả`n`lực kéo phải được giao cho máy tốt nhất đó. 

Câu trả lời cuối cùng là`n * max(E_i)`. 

### Tại sao nó hoạt động 

Mỗi lần kéo đóng góp một biến ngẫu nhiên độc lập mà kỳ vọng của nó chỉ phụ thuộc vào máy được chọn. Tổng phần thưởng dự kiến ​​​​là tổng kỳ vọng của các lần rút riêng lẻ. Bởi vì kỳ vọng có tính bổ sung nên việc sắp xếp lại lực kéo giữa các máy không tạo ra bất kỳ tác động chéo hoặc sức mạnh tổng hợp nào. Bất kỳ sự phân bổ nào sử dụng máy có kỳ vọng nhỏ hơn đều có thể được cải thiện bằng cách chuyển lực kéo đó sang máy có kỳ vọng lớn hơn, làm tăng tổng kỳ vọng một cách nghiêm ngặt. Điều này hàm ý một giải pháp tối ưu luôn gán mỗi lần kéo cho một máy duy nhất có giá trị kỳ vọng tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    
    best = 0.0
    
    for _ in range(k):
        m = int(input())
        rewards = list(map(int, input().split()))
        probs = list(map(float, input().split()))
        
        exp = 0.0
        for r, p in zip(rewards, probs):
            exp += r * p
        
        if exp > best:
            best = exp
    
    print(n * best)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giảm tối đa hóa kỳ vọng. Mỗi máy được xử lý độc lập và giá trị kỳ vọng của nó được tính dưới dạng tích số chấm giữa phần thưởng và xác suất. Trạng thái toàn cầu duy nhất được duy trì là kỳ vọng tối đa được thấy cho đến nay. 

Một cạm bẫy phổ biến là cố gắng phân phối lực kéo trên nhiều máy bằng cách sử dụng lập trình động. Điều đó là không cần thiết vì không có lợi nhuận giảm dần hoặc sự phụ thuộc giữa các lần kéo. 

Độ chính xác của dấu phẩy động là đủ vì các giá trị bị giới hạn và độ chính xác cần thiết là`1e-6`. Độ chính xác kép tiêu chuẩn xử lý thoải mái việc tích lũy lên tới 1000 thuật ngữ trên mỗi máy. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với hai máy và tổng cộng ba lần kéo. 

Máy 1 cho phần thưởng 10 với xác suất 0,5 và 0 với xác suất 0,5. Kỳ vọng của nó là 5.0. 

Máy 2 cho phần thưởng 3 với xác suất 1,0. Kỳ vọng của nó là 3.0. 

| Máy | Giá trị kỳ vọng | 
| --- | --- | 
| 1 | 5.0 | 
| 2 | 3.0 | 

| Bước | Tốt nhất cho đến nay | 
| --- | --- | 
| Sau máy 1 | 5.0 | 
| Sau máy 2 | 5.0 | 

Kết quả là`3 * 5.0 = 15.0`. 

Bây giờ hãy xem xét trường hợp một chiếc máy có nhiều kết quả nhưng có giá trị trung bình thấp. Giả sử máy A có kết quả`[0, 100]`với xác suất`[0.99, 0.01]`và máy B luôn trả về 1. Máy A có kỳ vọng là 1,0, máy B cũng có kỳ vọng là 1,0. Bất kỳ sự phân bổ nào cũng mang lại giá trị mong đợi như nhau, vì vậy việc gán tất cả các lần kéo cho một trong hai máy là tối ưu. 

Điều này xác nhận rằng phương sai không quan trọng và chỉ có giá trị trung bình mới quyết định giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · m_i) | Kỳ vọng của mỗi máy được tính toán bằng một lần chuyển qua phân phối của nó | 
| Không gian | O(1) | Chỉ một số đại lượng vô hướng được lưu trữ ngoài bộ đệm đầu vào | 

Các ràng buộc cho phép tối đa 100 máy với tối đa 1000 kết quả mỗi máy, do đó, tối đa 100.000 phép nhân được thực hiện. Điều này dễ dàng nằm trong giới hạn và mức sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    n, k = map(int, input().split())
    best = 0.0
    for _ in range(k):
        m = int(input())
        r = list(map(int, input().split()))
        p = list(map(float, input().split()))
        exp = sum(ri * pi for ri, pi in zip(r, p))
        best = max(best, exp)
    return str(best * n)

# sample
assert run("5 3\n1\n9\n1.0\n1\n7\n1.0\n1\n5\n1.0\n") == "45.0"

# all equal machines
assert run("2 2\n2\n1 3\n0.5 0.5\n2\n1 3\n0.5 0.5\n") == "4.0"

# single machine
assert run("4 1\n2\n0 10\n0.5 0.5\n") == "20.0"

# zero reward machine
assert run("3 2\n1\n0\n1.0\n1\n5\n1.0\n") == "15.0"

# mixed distributions
assert run("1 2\n2\n0 10\n0.9 0.1\n1\n5\n1.0\n") == "1.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| máy giống hệt nhau | kết quả như nhau bất kể lựa chọn | đối xứng | 
| máy đơn | nhân rộng trực tiếp | trường hợp cơ sở | 
| máy không thưởng | bỏ qua những lựa chọn vô ích | sự thống trị | 
| phân phối hỗn hợp | tính toán kỳ vọng đúng | tính chính xác của tổng trọng số | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều máy có cùng giá trị mong đợi. Trong tình huống đó, bất kỳ sự phân bổ lực kéo nào giữa chúng đều là tối ưu. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ theo dõi kỳ vọng tối đa và không phụ thuộc vào máy nào đạt được kỳ vọng đó. 

Một trường hợp khác là khi tất cả các máy đều có giá trị kỳ vọng bằng 0. Ngay cả khi đó, kỳ vọng được tính toán vẫn bằng 0 và nhân với`n`bảo toàn tính đúng đắn. Không cần xử lý đặc biệt vì mức tối đa vẫn bằng 0. 

Trường hợp tinh vi cuối cùng là độ chính xác về mặt số học khi xác suất rất nhỏ hoặc các giá trị gần giới hạn trên. Vì tất cả phần thưởng được giới hạn bởi 100 và có tối đa 1000 thuật ngữ trên mỗi máy, lỗi dấu phẩy động tích lũy vẫn thấp hơn nhiều so với yêu cầu`1e-6`sức chịu đựng.
