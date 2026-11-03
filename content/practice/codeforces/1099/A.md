---
title: "CF 1099A - Quả Cầu Tuyết"
description: "Quá trình này mô tả một quả cầu tuyết trượt xuống từ độ cao ban đầu cho đến khi chạm đất. Ở mọi cấp độ cao nguyên, quả cầu tuyết liên tục thay đổi trọng lượng của nó theo ba giai đoạn theo thứ tự: đầu tiên nó tăng thêm trọng lượng bằng với chiều cao hiện tại của nó, sau đó có thể…"
date: "2026-06-15T15:49:42+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1099
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 530 (Div. 2)"
rating: 800
weight: 1099
solve_time_s: 382
verified: true
draft: false
---

[CF 1099A - Quả cầu tuyết](https://codeforces.com/problemset/problem/1099/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 6 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình này mô tả một quả cầu tuyết trượt xuống từ độ cao ban đầu cho đến khi chạm đất. Ở mọi cấp độ cao nguyên, quả cầu tuyết liên tục thay đổi trọng lượng theo ba giai đoạn theo thứ tự: đầu tiên nó tăng thêm trọng lượng bằng chiều cao hiện tại, sau đó có thể giảm trọng lượng nếu có một hòn đá ở độ cao đó và cuối cùng di chuyển xuống một bậc. 

Đầu vào mô tả trạng thái ban đầu của hệ thống này. Chúng ta được biết trọng lượng ban đầu và chiều cao ban đầu của quả cầu tuyết và hai viên đá được đặt ở những độ cao cụ thể, mỗi viên đá sẽ giảm một lượng trọng lượng cố định bất cứ khi nào quả cầu tuyết đi qua chiều cao của chúng. 

Nhiệm vụ là mô phỏng chuyển động này cho đến khi quả cầu tuyết đạt độ cao bằng 0 và báo cáo trọng lượng cuối cùng của nó. Khó khăn chính là trọng lượng tiến triển liên tục với cả sự tích lũy và trừ đi ở các vị trí cụ thể. 

Các ràng buộc rất nhỏ: chiều cao nhiều nhất là 100 và trọng lượng nhiều nhất là 100. Điều này ngay lập tức ngụ ý rằng mô phỏng trực tiếp trên mỗi mức độ cao là đủ hiệu quả, vì tối đa 100 bước được thực hiện và mỗi bước thực hiện O(1). Bất kỳ giải pháp nào có tối đa khoảng 10^7 thao tác sẽ an toàn trong giới hạn thời gian, do đó không cần tối ưu hóa ngoài việc lặp lại đơn giản. 

Một số trường hợp đặc biệt dễ bị xử lý sai nếu trình tự thao tác không được tuân thủ chính xác. Người ta quên rằng trọng lượng tăng trước khi đá va chạm. Ví dụ: nếu một quả cầu tuyết bắt đầu ở độ cao 1 với một hòn đá ở độ cao 1, thì việc tăng dần chiều cao sẽ xảy ra trước tiên, do đó quả cầu tuyết sẽ tăng trọng lượng trước khi mất đi. Một trường hợp khác là xử lý sai trọng lượng âm sau khi va chạm với đá. Vấn đề rõ ràng là kẹp trọng số về 0 nhưng vẫn tiếp tục mô phỏng, do đó việc dừng sớm một cách ngây thơ sẽ không chính xác. 

Trường hợp tinh tế thứ ba là khi một hòn đá có trọng lượng bằng không. Ngay cả khi đó, va chạm vẫn phải được xử lý, mặc dù nó không thay đổi gì cả. Bỏ qua những viên đá như vậy có điều kiện là an toàn, nhưng bỏ qua hoàn toàn bước va chạm ở độ cao đó thì không, bởi vì việc sắp xếp thứ tự rất quan trọng để đảm bảo tính nhất quán. 

## Phương pháp tiếp cận 

Giải pháp brute-force mô phỏng trực tiếp quá trình từng giây một. Ở mỗi độ cao, chúng tôi tăng trọng lượng quả cầu tuyết theo độ cao hiện tại, gây sát thương bằng đá nếu có, giữ kết quả về 0 nếu âm và sau đó giảm độ cao. Vì chiều cao giảm đúng một lần trong mỗi lần lặp nên mô phỏng này chạy đúng h bước. Mỗi bước có thời gian không đổi, vì vậy cách tiếp cận vũ phu đã tối ưu với các ràng buộc nhất định. 

Không có cấu trúc toán học ẩn nào để nén thêm vì quá trình chuyển đổi trạng thái chỉ phụ thuộc vào chiều cao và cân nặng hiện tại, còn chiều cao giảm nghiêm ngặt. Bất kỳ nỗ lực bỏ qua các bước nào vẫn sẽ yêu cầu tính toán các tương tác đá ở từng độ cao riêng lẻ, do đó, không có thủ thuật tổng hợp nào làm giảm độ phức tạp hơn nữa theo cách có ý nghĩa. 

Sự khác biệt chính giữa các giải pháp đúng và sai không phải là hiệu quả mà là việc tuân thủ nghiêm ngặt thứ tự sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(h) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(h) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quả cầu tuyết rơi xuống từng cấp một, luôn theo dõi chiều cao và cân nặng hiện tại của nó.

1. Khởi tạo trọng lượng và chiều cao của quả cầu tuyết từ đầu vào và lưu trữ hai viên đá trong cấu trúc cho phép tra cứu nhanh theo chiều cao. Điều này quan trọng vì chúng ta cần kiểm tra liên tục ở mỗi bước. 
2. Khi chiều cao hiện tại lớn hơn 0, hãy thực hiện một chu trình cập nhật đầy đủ. 
3. Đầu tiên hãy tăng trọng lượng quả cầu tuyết lên theo chiều cao hiện tại. Bước này rất quan trọng vì vấn đề xác định sự tăng cân trước khi có bất kỳ tương tác nào với sỏi. Thứ tự quyết định tính đúng đắn. 
4. Kiểm tra xem hòn đá có tồn tại ở độ cao hiện tại hay không. Nếu có, hãy trừ đi trọng lượng của nó khỏi quả cầu tuyết. Nếu có nhiều viên đá ở cùng độ cao, cả hai hiệu ứng sẽ được áp dụng tuần tự, mặc dù dữ liệu đầu vào đảm bảo độ cao khác biệt ở đây. 
5. Nếu trọng lượng thu được trở nên âm, hãy kẹp nó về 0. Quả cầu tuyết không dừng lại hoặc thay đổi hành vi; chỉ có trọng lượng được lưu trữ được điều chỉnh. 
6. Giảm chiều cao xuống một đơn vị để mô phỏng chuyển động đi xuống. 
7. Lặp lại cho đến khi chiều cao bằng 0, sau đó xuất ra trọng lượng cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, thuật toán duy trì trạng thái chính xác được mô tả trong định nghĩa bài toán: trọng lượng phản ánh tất cả các bước tăng trước đó và tương tác của đá ở mức cao hơn hoặc bằng nhau, đồng thời chiều cao theo dõi chặt chẽ các mức chưa được xử lý còn lại. Vì mỗi chiều cao được xử lý chính xác một lần và theo đúng thứ tự nên không có đóng góp nào bị bỏ qua hoặc tính hai lần. Do đó, trạng thái cuối cùng sau độ cao bằng 0 là kết quả tích lũy của tất cả các chuyển đổi xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    w, h = map(int, input().split())
    u1, d1 = map(int, input().split())
    u2, d2 = map(int, input().split())

    stone = {}
    stone[d1] = u1
    stone[d2] = u2

    weight = w
    height = h

    while height > 0:
        weight += height

        if height in stone:
            weight -= stone[height]
            if weight < 0:
                weight = 0

        height -= 1

    print(weight)

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ các hiệu ứng của đá trong một từ điển được khóa theo chiều cao để việc kiểm tra đá ở một cấp độ nhất định là O(1). Vòng lặp xử lý độ cao từ h xuống 1, phù hợp với chuyển động vật lý của quả cầu tuyết. Bước tăng xảy ra trước khi tra cứu, giữ nguyên thứ tự cần thiết. 

Bước kẹp sau khi trừ là cần thiết vì các trọng số âm không được phép tồn tại, mặc dù quá trình vẫn tiếp tục không thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 1
1 2
```| Chiều cao | Trọng lượng bắt đầu | + Chiều cao | Đá Mất | Sau Kẹp | Chiều cao tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 7 | 0 | 7 | 2 | 
| 2 | 7 | 9 | 1 | 8 | 1 | 
| 1 | 8 | 9 | 1 | 8 | 0 | 

Dấu vết này cho thấy mỗi chiều cao góp phần chính xác một lần vào việc tăng cân và các tương tác đá chỉ được áp dụng ở mức độ được chỉ định. Trọng số cuối cùng phù hợp với sản lượng dự kiến là 8. 

### Ví dụ 2 

đầu vào:```
5 4
3 4
2 2
```| Chiều cao | Trọng lượng bắt đầu | + Chiều cao | Đá Mất | Sau Kẹp | Chiều cao tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 5 | 9 | 3 | 6 | 3 | 
| 3 | 6 | 9 | 0 | 9 | 2 | 
| 2 | 9 | 11 | 2 | 9 | 1 | 
| 1 | 9 | 10 | 0 | 10 | 0 | 

Trường hợp này nhấn mạnh rằng các viên đá ở các độ cao khác nhau điều chỉnh quỹ đạo một cách độc lập mà không tương tác và thứ tự các hoạt động quyết định sự tích lũy cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Mỗi cấp độ cao được xử lý chính xác một lần với các thao tác có thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi và một bản đồ nhỏ cho hai viên đá | 

Chiều cao tối đa là 100, do đó mô phỏng thực hiện tối đa 100 lần lặp, nằm trong giới hạn ngay cả dưới những ràng buộc nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    w, h = map(int, input().split())
    u1, d1 = map(int, input().split())
    u2, d2 = map(int, input().split())

    stone = {d1: u1, d2: u2}
    weight = w

    while h > 0:
        weight += h
        if h in stone:
            weight -= stone[h]
            if weight < 0:
                weight = 0
        h -= 1

    return str(weight)

assert run("4 3\n1 1\n1 2\n") == "8"

assert run("0 1\n0 1\n0 1\n") == "1"

assert run("10 5\n0 3\n0 4\n") == "25"

assert run("5 4\n5 4\n0 2\n") == "9"

assert run("1 2\n10 1\n10 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chiều cao tối thiểu | 1 | Độ chính xác của mô phỏng một bước | 
| Không có trọng lượng đá | Chỉ tăng trưởng | Xử lý đá không cần op | 
| Nhiều chiều cao trung bình | Tích lũy đúng đắn | Hành vi phụ gia thích hợp | 
| Đá nặng lúc đầu | Kẹp ngay lập tức | Xử lý trọng lượng tiêu cực | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi hòn đá ở độ cao giảm bớt trọng lượng nhiều hơn hiện tại. Ví dụ, ở độ cao mà quả cầu tuyết vừa mới đạt được một lượng nhỏ, một hòn đá lớn có thể đẩy nó âm. Thuật toán kẹp rõ ràng về 0 sau khi trừ, đảm bảo trạng thái vẫn hợp lệ mà không làm gián đoạn vòng lặp. 

Một trường hợp khác là khi một hòn đá ở độ cao 1. Dù đây là bước hoạt động cuối cùng nhưng quả cầu tuyết vẫn thực hiện đầy đủ trình tự: tăng 1, đặt đá, sau đó di chuyển về 0. Cấu trúc vòng lặp xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt vì chiều cao chỉ trở thành 0 sau khi xử lý. 

Trường hợp thứ ba là khi cả hai viên đá đều có trọng lượng bằng không. Mô phỏng vẫn thực hiện tra cứu từ điển và thử trừ, nhưng hiệu quả thực sự là trung tính. Tính chính xác phụ thuộc vào việc vẫn thực hiện bước xung đột, ngay cả khi nó không thay đổi trạng thái.
