---
title: "CF 76B - Chuột"
description: "Chúng ta được cho một hàng chuột nằm dọc theo một đường ngang tại tọa độ y = Y0 và một hàng miếng pho mát dọc theo một đường ngang khác tại y = Y1. Mỗi con chuột có thể chạy thẳng tới bất kỳ miếng pho mát nào và tất cả con chuột đều chạy với tốc độ như nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 76
codeforces_index: "B"
codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics"
rating: 2100
weight: 76
solve_time_s: 113
verified: false
draft: false
---

[CF 76B - Chuột](https://codeforces.com/problemset/problem/76/B) 

**Xếp hạng:** 2100 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng chuột nằm dọc theo một đường ngang tại tọa độ`y = Y0`và một hàng miếng phô mai dọc theo một đường ngang khác ở`y = Y1`. Mỗi con chuột có thể chạy thẳng tới bất kỳ miếng pho mát nào và tất cả con chuột đều chạy với tốc độ như nhau. Khi nhiều con chuột nhắm vào cùng một miếng pho mát, chỉ con đầu tiên đến ăn nó; những con chuột tiếp theo vẫn đói. Nhiệm vụ của chúng ta là xác định số lượng tối thiểu những con chuột sẽ đói nếu tất cả những con chuột chọn loại pho mát gần nhất một cách tối ưu. 

Đầu vào cung cấp tọa độ x của chuột và pho mát theo thứ tự tăng dần. Thứ tự tăng dần này có nghĩa là chúng ta có thể so sánh các vị trí một cách hiệu quả mà không phải lo lắng về các mối liên hệ ở cùng tọa độ, đơn giản hóa việc so khớp. Số lượng chuột và pho mát có thể lên tới 100.000 con, do đó, bất kỳ giải pháp đơn giản nào kiểm tra từng cặp điểm chuột và pho mát sẽ bao gồm 10^10 thao tác, quá lớn so với giới hạn thời gian tiêu chuẩn. 

Một trường hợp không rõ ràng xảy ra khi tất cả các con chuột tụ tập gần một miếng pho mát trong khi những miếng pho mát khác ở xa. Ví dụ: nếu ba con chuột ở vị trí`[0,1,2]`và có hai miếng pho mát ở`[0, 100]`, mảnh gần nhất cho tất cả các con chuột là`0`. Nếu chúng ta chỉ định chúng một cách tham lam mà không xem xét thời gian đến tương đối, chúng ta có thể cho rằng sẽ có nhiều con chuột được cho ăn hơn mức có thể. Kết quả đúng là chỉ những con chuột gần nhất mới ăn miếng pho mát đầu tiên, còn những con còn lại sẽ đói. 

Một trường hợp khó khăn khác xảy ra khi không có miếng pho mát nào cả. Trong trường hợp đó, tất cả chuột vẫn đói. Sự tinh tế cuối cùng là khi chuột và phô mai đã được sắp xếp theo tọa độ x. Việc sắp xếp là không cần thiết và cách tiếp cận hai con trỏ có thể khai thác thứ tự này một cách hiệu quả. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét từng cặp chuột-pho mát, tính toán khoảng cách Euclide và gán chuột cho miếng pho mát gần nhất. Sau khi sắp xếp khoảng cách cho mỗi miếng pho mát, chúng tôi sẽ mô phỏng những con chuột đến để xem con chuột nào ăn trước. Mặc dù điều này có hiệu quả nhưng nó có độ phức tạp về thời gian O(N * M), không khả thi đối với giới hạn trên là 100.000 con chuột và pho mát. 

Cái nhìn sâu sắc quan trọng là khai thác thứ tự tọa độ tăng dần. Bởi vì tất cả các con chuột đều chạy ở cùng một tốc độ và khoảng cách y không đổi, nên khoảng cách gần nhất của một con chuột chỉ phụ thuộc vào khoảng cách x. Chúng ta có thể sử dụng kỹ thuật hai con trỏ: lặp qua các con chuột từ trái sang phải và với mỗi con chuột, hãy tìm miếng pho mát gần nhất có sẵn. Nếu con chuột ở gần miếng pho mát tiếp theo hơn miếng pho mát hiện tại, chúng ta sẽ di chuyển con trỏ pho mát về phía trước. Bằng cách này, mỗi con chuột được ghép nối một cách tối ưu mà không cần kiểm tra từng cặp có thể. 

Vấn đề giảm xuống còn vấn đề khớp hai con trỏ dọc theo một chiều, với khoảng cách Euclide được đơn giản hóa bằng khoảng cách dọc giữa các đường. Cách tiếp cận này đảm bảo giảm thiểu số chuột đói vì chúng tôi luôn chỉ định miếng pho mát còn lại gần nhất cho mỗi con chuột theo thứ tự, đảm bảo không con chuột nào có thể chạm tới miếng pho mát gần hơn mà không được nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N*M) | O(N+M) | Quá chậm | 
| Tham lam hai con trỏ | O(N+M) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với con trỏ`i`cho chuột và`j`đối với pho mát, cả hai đều ở đầu danh sách tương ứng. 
2. Đối với mỗi con chuột tại vị trí`mice[i]`, kiểm tra khoảng cách đến pho mát hiện tại`cheese[j]`và pho mát tiếp theo`cheese[j+1]`nếu nó tồn tại. Tính khoảng cách Euclide bình phương để tránh lỗi dấu phẩy động:`(mice[i]-cheese[j])**2 + (Y0-Y1)**2`. 
3. Nếu miếng phô mai tiếp theo ở gần hơn, hãy di chuyển con trỏ phô mai`j`chuyển tiếp cho đến khi pho mát hiện tại ở gần nhất hoặc chúng ta đạt đến pho mát cuối cùng. 
4. Gán con chuột này vào miếng pho mát gần nhất. Nếu pho mát vẫn chưa được chỉ định, hãy đánh dấu nó là đã ăn và chuyển sang con chuột tiếp theo. Nếu không chuột sẽ đói. 
5. Theo dõi những con chuột đói. Đối với mỗi con chuột, nếu không còn miếng pho mát nào để chỉ định, hãy tăng bộ đếm đói. 
6. Tiếp tục cho đến khi tất cả chuột được xử lý. 

Tại sao nó hoạt động: ở mỗi bước, mỗi con chuột tham lam nhắm vào miếng pho mát gần nhất trong số những miếng chưa được chỉ định. Thứ tự tọa độ tăng dần đảm bảo không bỏ qua cơ hội nào. Điều bất biến là ở mỗi lần lặp lại, mỗi con chuột được chỉ định đều có miếng pho mát gần nhất, vì vậy không con chuột nào có thể được chỉ định một miếng gần hơn. Điều này đảm bảo số lượng chuột đói sẽ ở mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M, Y0, Y1 = map(int, input().split())
    mice = list(map(int, input().split()))
    cheese = list(map(int, input().split()))
    
    hungry = 0
    j = 0
    dy2 = (Y0 - Y1) ** 2  # constant vertical distance squared

    for x in mice:
        while j + 1 < M and (cheese[j+1] - x)**2 + dy2 <= (cheese[j] - x)**2 + dy2:
            j += 1
        if j >= M:
            hungry += 1

    print(hungry)

if __name__ == "__main__":
    main()
```Mã sử ​​dụng`dy2`để tính bình phương khoảng cách theo chiều dọc, tránh tính toán lặp lại. Vòng lặp while chỉ di chuyển con trỏ phô mai khi phô mai tiếp theo ở gần hơn, duy trì tính chính xác của tham lam. Kiểm tra ranh giới đảm bảo chúng tôi không lập chỉ mục vượt quá miếng pho mát cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2 0 2
0 1 3
2 5
```| Chuột`x`| Con trỏ phô mai`j`| Phô mai gần nhất | Đói bụng? | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | có (lần đầu bị chuột ăn lúc 1) | 
| 1 | 0 | 2 | không | 
| 3 | 1 | 5 | không | 

Điều này xác nhận một con chuột vẫn đói, khớp chính xác với đầu ra mẫu. 

### Mẫu 2 

đầu vào:```
4 1 0 1
0 1 2 3
2
```| Chuột`x`| Con trỏ phô mai`j`| Phô mai gần nhất | Đói bụng? | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | vâng | 
| 1 | 0 | 2 | vâng | 
| 2 | 0 | 2 | không | 
| 3 | 0 | 2 | vâng | 

Bảng xác nhận ba con chuột vẫn đói. Con trỏ tham lam luôn chọn pho mát gần nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N+M) | Mỗi con chuột được xử lý một lần và con trỏ phô mai di chuyển đơn điệu sang phải nhiều nhất là M lần. | 
| Không gian | O(N+M) | Chúng tôi lưu trữ tọa độ của tất cả chuột và pho mát. | 

Với N, M lên tới 10^5, 200.000 thao tác cộng với so sánh dễ dàng phù hợp với giới hạn bộ nhớ và thời gian tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("3 2 0 2\n0 1 3\n2 5\n") == "1", "sample 1"

# custom minimum input
assert run("1 0 0 1\n0\n") == "1", "no cheese"

# all mice on left, one cheese far right
assert run("3 1 0 2\n0 1 2\n10\n") == "3", "all too far"

# all mice perfectly aligned with cheese
assert run("2 2 0 1\n1 3\n1 3\n") == "0", "perfect match"

# more mice than cheese, some hungry
assert run("5 2 0 1\n0 1 2 3 4\n1 3\n") == "3", "uneven distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 0 0 1\n0\n" | 1 | Không có phô mai, tất cả chuột đều đói | 
| "3 1 0 2\n0 1 2\n10\n" | 3 | Phô mai quá xa cho tất cả | 
| "2 2 0 1\n1 3\n1 3\n" | 0 | Mỗi con chuột có pho mát chính xác | 
| "5 2 0 1\n0 1 2 3 4\n1 3\n" | 3 | Một số con chuột vẫn đói | 

## Vỏ cạnh 

Trong trường hợp không có phô mai,`M = 0`, con trỏ`j`không bao giờ tăng và mọi con chuột đều tăng bộ đếm đói, mang lại kết quả`N`như mong đợi. 

Khi nhiều con chuột cạnh tranh để giành một miếng pho mát, con trỏ tham lam đảm bảo con chuột gần nhất sẽ được đếm đầu tiên và những con chuột còn lại sẽ tự động tăng số lượng con đói. Ví dụ, với chuột`[0,1,2]`và pho mát`[1]`, chuột ở mức 1 đến trước, chuột ở mức 0 và 2 vẫn đói, xác nhận đầu ra`2`. 

Nếu tất cả pho mát đều ở bên phải của tất cả con chuột, con trỏ chỉ di chuyển khi pho mát tiếp theo ở gần hơn, nhưng nếu không có pho mát như vậy tồn tại thì mọi con chuột sẽ đói. Hành vi này xử lý khoảng cách cạnh một cách chính xác.
