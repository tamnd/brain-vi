---
title: "CF 104636C - Thứ hạng"
description: "Chúng ta được cung cấp một danh sách sinh viên, mỗi sinh viên được xác định bằng một số nguyên id từ 1 đến n. Học sinh 1 là Thomas. Mỗi học sinh có bốn điểm thi và thành tích tổng thể của họ được đo bằng tổng của bốn điểm này."
date: "2026-06-29T17:05:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 83
verified: false
draft: false
---

[CF 104636C - Xếp hạng](https://codeforces.com/problemset/problem/104636/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách sinh viên, mỗi sinh viên được xác định bằng một số nguyên id từ 1 đến n. Học sinh 1 là Thomas. Mỗi học sinh có bốn điểm thi và thành tích tổng thể của họ được đo bằng tổng của bốn điểm này. 

Tất cả học sinh được xếp hạng bằng cách sắp xếp chúng theo thứ tự giảm dần của tổng số điểm này. Nếu hai học sinh có tổng điểm bằng nhau thì việc so điểm sẽ bị phá vỡ bằng cách chọn học sinh có id nhỏ hơn trước. Nhiệm vụ là xác định vị trí cuối cùng của Thomas trong trật tự này. 

Các ràng buộc n 1000 và giá trị điểm lên tới 100 có nghĩa là mỗi tổng điểm được giới hạn bằng 400. Điều này ngay lập tức gợi ý rằng mọi giải pháp O(n^2) hoặc O(n log n) đều nhanh chóng, vì thậm chí 10^6 phép so sánh cũng không đáng kể trong 1 giây. 

Sự tinh tế chính là quy tắc ràng buộc. Một lỗi phổ biến là chỉ tính điểm cao hơn và quên rằng điểm bằng nhau vẫn yêu cầu id so sánh. Một trường hợp thất bại khác là chỉ sắp xếp theo điểm mà không mã hóa thứ tự id chính xác, dẫn đến vị trí không chính xác khi tồn tại trùng lặp. 

Một trường hợp cụ thể là khi nhiều học sinh có cùng tổng điểm như Thomas. Ví dụ: nếu Thomas có tổng số 390 và hai học sinh khác cũng có 390, thì chỉ những học sinh có điểm cao hơn mới được xếp trước anh ấy và trong số các điểm bằng nhau, Thomas sẽ đứng đầu do id = 1. Việc thực hiện bất cẩn tính “điểm ≥ điểm Thomas” vì thứ hạng cao hơn sẽ đẩy Thomas xuống một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính tổng điểm của mỗi học sinh, sau đó sắp xếp các học sinh đó bằng cách sử dụng quy tắc sắp xếp bắt buộc và cuối cùng xác định vị trí của học sinh 1 trong danh sách đã sắp xếp. 

Điều này có hiệu quả vì định nghĩa xếp hạng chính xác là một vấn đề về thứ tự tổng thể: khi tất cả các tổng được tính toán, nhiệm vụ sẽ giảm xuống việc sắp xếp các cặp (điểm, id) theo thứ tự từ điển trong đó điểm giảm dần và id tăng dần. 

Một giải pháp thay thế mạnh mẽ là so sánh Thomas với mọi học sinh khác và đếm xem có bao nhiêu học sinh xếp trước anh ta. Học sinh dẫn đầu nếu điểm của họ lớn hơn điểm của Thomas hoặc nếu điểm bằng nhau và id của họ nhỏ hơn. Vì Thomas có id nhỏ nhất nên không có học sinh có điểm bằng nhau nào có thể xếp hạng cao hơn anh ta, vì vậy chỉ có điểm lớn hơn mới quan trọng. Quan sát này làm giảm logic đáng kể. 

Ý tưởng bạo lực đã chạy trong O(n), nhưng việc khái quát hóa nó cho bất kỳ học sinh nào sẽ yêu cầu so sánh O(n^2). Cách tiếp cận dựa trên sắp xếp thống nhất hơn và gần hơn với cách triển khai các hệ thống xếp hạng thường được triển khai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Đọc tất cả học sinh và tính tổng điểm của họ bằng cách cộng bốn môn học. 

Điều này biến vấn đề thành giải quyết với một giá trị có thể so sánh duy nhất cho mỗi học sinh. 
2. Lưu trữ mỗi học sinh thành một cặp chứa tổng điểm và id của họ. 

Chúng tôi giữ id vì việc hòa giải phụ thuộc vào nó. 
3. Sắp xếp danh sách sinh viên theo hai từ khóa: đầu tiên là giảm tổng điểm, sau đó là tăng id. 

Điều này trực tiếp mã hóa quy tắc xếp hạng vào bộ so sánh sắp xếp. 
4. Quét qua danh sách đã sắp xếp và tìm vị trí có id bằng 1. 

Chỉ mục theo thứ tự được sắp xếp này là câu trả lời, được điều chỉnh thành chỉ mục dựa trên 1. 

### Tại sao nó hoạt động 

Sau khi tính tổng, mọi so sánh giữa các học sinh chỉ phụ thuộc vào hai giá trị: điểm và id. Thứ tự sắp xếp khớp chính xác với quy tắc xếp hạng của bài toán, nghĩa là mảng được sắp xếp là thứ hạng cuối cùng hợp lệ. Vì việc sắp xếp tạo ra tổng thứ tự phù hợp với hàm so sánh được yêu cầu nên vị trí của học sinh 1 được đảm bảo phù hợp với thứ hạng của họ. Không cần điều chỉnh sau này vì tất cả sự ràng buộc đã được giải quyết trong quá trình sắp xếp. 

## Giải pháp Python

```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    students = []
    
    for i in range(1, n + 1):
        a, b, c, d = map(int, input().split())
        total = a + b + c + d
        students.append(( -total, i ))  # negative for descending sort
    
    students.sort()
    
    for idx, (_, sid) in enumerate(students, start=1):
        if sid == 1:
            print(idx)
            return

if __name__ == "__main__":
    main()
```Việc triển khai sử dụng điểm phủ định để tránh viết một bộ so sánh tùy chỉnh: Python sắp xếp tăng dần theo mặc định, do đó việc lật dấu sẽ đạt được thứ tự giảm dần. Id được giữ nguyên để các mối quan hệ có thể giải quyết một cách tự nhiên trước các id nhỏ hơn. 

Vòng lặp trên các học sinh được sắp xếp là an toàn vì n ≤ 1000, do đó việc quét tuyến tính là không đáng kể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Học sinh đầu vào đưa ra tổng số: 398, 400, 398, 379, 357. Chúng tôi theo dõi việc sắp xếp theo (-score, id). 

| Bước | Sinh viên | Tổng cộng | Khóa (-total, id) | Sắp xếp thứ tự cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 398 | (-398, 1) | | 
| 2 | 2 | 400 | (-400, 2) | | 
| 3 | 3 | 398 | (-398, 3) | | 
| 4 | 4 | 379 | (-379, 4) | | 
| 5 | 5 | 357 | (-357, 5) | | 

Sau khi sắp xếp, thứ tự là học sinh 2, rồi đến học sinh 1 và 3, rồi 4, rồi 5. Học sinh 1 ở vị trí thứ hai nên kết quả là 2. 

Dấu vết này cho thấy các tổng bằng nhau được giải quyết hoàn toàn theo thứ tự id bên trong cấu trúc được sắp xếp như thế nào. 

### Mẫu 2 

Tổng số là 369, 240, 310, 300, 300, 0. 

| Bước | Sinh viên | Tổng cộng | Khóa (-total, id) | Sắp xếp thứ tự cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 369 | (-369, 1) | | 
| 2 | 2 | 240 | (-240, 2) | | 
| 3 | 3 | 310 | (-310, 3) | | 
| 4 | 4 | 300 | (-300, 4) | | 
| 5 | 5 | 300 | (-300, 5) | | 
| 6 | 6 | 0 | (0, 6) | | 

Thứ tự sắp xếp bắt đầu từ học sinh 1, vì 369 là lớn nhất. Vì vậy Thomas là người đầu tiên. 

Điều này xác nhận rằng khi không có ai vượt quá số điểm của Thomas, thứ hạng của anh ta là 1 bất kể các cách phân bổ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp n sinh viên chiếm ưu thế trong mọi công việc | 
| Không gian | O(n) | lưu trữ danh sách các bộ dữ liệu sinh viên | 

Các ràng buộc n 1000 làm cho việc này nhanh chóng một cách thoải mái. Ngay cả trong trường hợp xấu nhất, việc sắp xếp 1000 phần tử là chuyện nhỏ và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    try:
        main()
    except SystemExit:
        pass
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n100 98 100 100\n100 100 100 100\n100 100 99 99\n90 99 90 100\n100 98 60 99\n") == "2"
assert run("6\n100 80 90 99\n60 60 60 60\n90 60 100 60\n60 100 60 80\n100 100 0 100\n0 0 0 0\n") == "1"

# all equal
assert run("3\n10 10 10 10\n10 10 10 10\n10 10 10 10\n") == "1"

# Thomas already lowest
assert run("3\n0 0 0 0\n100 100 100 100\n50 50 50 50\n") == "3"

# maximum tie around Thomas
assert run("4\n10 10 10 10\n10 10 10 10\n10 10 10 10\n10 10 10 10\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có điểm bằng nhau | 1 | hòa theo id vị trí Thomas đầu tiên | 
| Thomas thấp nhất | 3 | đúng thứ hạng khi người khác thống trị | 
| cà vạt đồng phục | 1 | xử lý ổn định sự bình đẳng hoàn toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả học sinh có tổng điểm giống nhau. Trong tình huống này, việc sắp xếp hoàn toàn dựa vào thứ tự id. Vì học sinh 1 có id nhỏ nhất nên thứ tự sắp xếp bắt đầu bằng Thomas và thuật toán trả về chính xác thứ hạng 1. 

Một trường hợp khác là khi Thomas có điểm thấp nhất có thể, chẳng hạn như toàn số 0 trong khi những người khác có điểm tối đa. Việc sắp xếp đặt anh ấy ở cuối vì mọi học sinh khác đều có điểm cao hơn (-score âm hơn), do đó thứ hạng của anh ấy trở thành n, phù hợp với mong đợi. 

Một trường hợp tinh vi cuối cùng là khi nhiều học sinh có cùng điểm với Thomas nhưng không có học sinh nào vượt quá nó. Việc sắp xếp đảm bảo Thomas xuất hiện trước tất cả bọn họ do thứ tự id, vì vậy thứ hạng của anh ấy vẫn là 1 mặc dù tồn tại nhiều mối quan hệ.
