---
title: "CF 105315J - Sinh Nhật Hamza"
description: "Mỗi trường hợp thử nghiệm mô tả một tập hợp các món ăn, trong đó mỗi món ăn thuộc về một nhà hàng cụ thể và có giá trị hương vị bằng số. Hạn chế chính là bạn không thể kết hợp các nhà hàng. Bạn phải chọn chính xác một nhà hàng và lấy tất cả các món ăn của nhà hàng đó."
date: "2026-06-23T06:15:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "J"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 62
verified: true
draft: false
---

[CF 105315J - Sinh nhật của Hamza](https://codeforces.com/problemset/problem/105315/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một tập hợp các món ăn, trong đó mỗi món ăn thuộc về một nhà hàng cụ thể và có giá trị hương vị bằng số. Hạn chế chính là bạn không thể kết hợp các nhà hàng. Bạn phải chọn chính xác một nhà hàng và lấy tất cả các món ăn của nhà hàng đó. Giá trị của một nhà hàng được chọn được xác định bởi món ăn yếu nhất trong đó, nghĩa là giá trị hương vị tối thiểu trong số tất cả các món ăn của nhà hàng đó. Nhiệm vụ của bạn là xác định nhà hàng có món ăn yếu nhất càng mạnh càng tốt. 

Nói một cách cụ thể hơn, đối với mỗi nhãn nhà hàng, bạn nhóm tất cả các giá trị hương vị liên quan của nó, tính giá trị tối thiểu trong nhóm đó và sau đó trong số tất cả các giá trị tối thiểu này, bạn chọn giá trị tối đa. 

Kích thước đầu vào lên tới tổng cộng 500.000 món ăn trong tất cả các trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi phương pháp liên tục quét danh sách cho mỗi truy vấn hoặc tính toán lại mức tối thiểu từ đầu cho mỗi lựa chọn nhà hàng. Một giải pháp bậc hai về số lượng món ăn sẽ quá chậm vì ngay cả 10^5 thao tác trên mỗi trường hợp thử nghiệm được lặp lại 10^3 lần cũng đã dẫn đến thời gian chạy không thể chấp nhận được. 

Một trường hợp phức tạp xuất hiện khi nhiều món ăn của cùng một nhà hàng nhưng được sắp xếp theo thứ tự tùy ý. Ví dụ: nếu một nhà hàng có các món ăn có giá trị 10, 2 và 7 thì đóng góp câu trả lời từ nhà hàng này là 2, bất kể thứ tự món ăn. Một cách tiếp cận đơn giản chỉ theo dõi giá trị nhìn thấy lần cuối của mỗi nhà hàng sẽ xuất sai 7 trong trường hợp này nếu giá trị nhỏ nhất xuất hiện sớm hơn và bị ghi đè. 

Một trường hợp khác phát sinh khi mỗi nhà hàng chỉ có đúng một món ăn. Khi đó, câu trả lời đơn giản là di tối đa, vì giá trị tối thiểu của mỗi nhà hàng là giá trị duy nhất. Bất kỳ logic nhóm không chính xác nào vô tình hợp nhất các nhà hàng sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là đối xử độc lập với từng nhà hàng. Đối với mỗi nhãn nhà hàng, chúng tôi thu thập tất cả các món ăn của nó và tính toán rõ ràng mức tối thiểu bằng cách quét danh sách đầy đủ các món ăn được chỉ định cho nó. Nếu có n món ăn và có thể có n nhà hàng riêng biệt thì trong trường hợp xấu nhất, chúng tôi liên tục quét lại gần như toàn bộ mảng cho mỗi nhà hàng. Điều này dẫn đến số lượng hoạt động trong trường hợp xấu nhất theo thứ tự n bình phương, trở thành khoảng 10^10 hoạt động ở mức giới hạn tối đa, vượt xa giới hạn 2 giây có thể xử lý. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải tính toán lại bất cứ điều gì. Mỗi món ăn đóng góp chính xác một giá trị cho đúng một nhóm nhà hàng và thông tin duy nhất quan trọng đối với mỗi nhóm là thông tin tối thiểu. Điều này gợi ý việc duy trì mức hoạt động tối thiểu cho mỗi nhà hàng khi chúng tôi đọc dữ liệu đầu vào một lần. Thay vì truy cập lại dữ liệu, chúng tôi cập nhật một từ điển được khóa theo id nhà hàng, lưu trữ giá trị nhỏ nhất được thấy cho đến nay. 

Điều này biến vấn đề thành một vấn đề tổng hợp một lượt. Mỗi lần cập nhật là thời gian không đổi, sau khi xử lý xong tất cả các món ăn, chúng ta chỉ cần quét mức tối thiểu đã thu thập để tìm mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập, xây dựng ánh xạ từ id nhà hàng đến giá trị món ăn tối thiểu hiện tại.

1. Khởi tạo một từ điển trống sẽ lưu trữ giá trị hương vị nhỏ nhất được thấy cho đến nay đối với mỗi nhà hàng. Cấu trúc này là cần thiết vì chúng ta cần cập nhật liên tục cho mỗi món ăn. 
2. Đọc từng món một. Đối với món ăn của nhà hàng a có giá trị d, hãy kiểm tra xem a đã được nhìn thấy trước đó chưa. Nếu không, hãy lưu trữ d trực tiếp ở mức tối thiểu hiện tại. 
3. Nếu nhà hàng đã tồn tại trong từ điển, chỉ cập nhật giá trị được lưu trữ nếu món ăn mới có giá trị nhỏ hơn. Điều này đảm bảo chúng ta không bao giờ mất dấu món ăn yếu nhất trong nhà hàng đó. 
4. Sau khi xử lý tất cả các món ăn, lặp lại tất cả các mức tối thiểu được lưu trữ và tính mức tối đa trong số đó. Bước cuối cùng này chọn nhà hàng tốt nhất có thể theo tiêu chí yêu cầu. 
5. Xuất giá trị lớn nhất này cho ca kiểm thử. 

Lý do điều này có hiệu quả là vì sự đóng góp của mỗi nhà hàng được xác định đầy đủ bởi giá trị tối thiểu của nó và mức tối thiểu đó có thể được duy trì tăng dần mà không cần phải xem lại danh sách đầy đủ các món ăn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình chế biến, từ điển sẽ lưu trữ chính xác số lượng món ăn tối thiểu đã chế biến cho mỗi nhà hàng. Đây là một bất biến được bảo toàn bằng cách chỉ thay thế các giá trị được lưu trữ khi một giá trị nhỏ hơn xuất hiện. Vì mỗi món ăn được chế biến chính xác một lần nên khi kết thúc dữ liệu đầu vào, giá trị được lưu trữ của mỗi nhà hàng chính xác là mức tối thiểu chung của toàn bộ món ăn của nhà hàng đó. Tận dụng tối đa các mức tối thiểu chính xác này sẽ mang lại sự lựa chọn nhà hàng tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        best = {}
        for _ in range(n):
            a, d = map(int, input().split())
            if a in best:
                if d < best[a]:
                    best[a] = d
            else:
                best[a] = d

        ans = 0
        for v in best.values():
            if v > ans:
                ans = v
        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là từ điển`best`, lưu trữ giá trị tối thiểu hiện tại cho mỗi nhà hàng. Bước cập nhật được viết cẩn thận để tránh việc gọi hoặc hợp nhất hàm không cần thiết. Vòng lặp cuối cùng là riêng biệt vì việc kết hợp việc tổng hợp với việc đọc sẽ làm phức tạp khả năng suy luận về tính chính xác và có nguy cơ thiếu kết quả so sánh cuối cùng trên tất cả các nhà hàng. 

Một điểm tinh tế là khởi tạo`ans`. Bắt đầu từ 0 là an toàn vì tất cả các giá trị món ăn đều dương như được đưa ra trong các ràng buộc. Nếu các giá trị có thể âm, việc khởi tạo sẽ cần được điều chỉnh thành âm vô cực hoặc giá trị từ điển đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
4 5
10 7
4 6
10 2
```Chúng tôi theo dõi các yêu cầu tối thiểu của nhà hàng từng bước. 

| Bước | Nhà hàng | Giá trị | Trạng thái lưu trữ | 
| --- | --- | --- | --- | 
| 1 | 4 | 5 | {4: 5} | 
| 2 | 10 | 7 | {4: 5, 10: 7} | 
| 3 | 4 | 6 | {4: 5, 10: 7} | 
| 4 | 10 | 2 | {4: 5, 10: 2} | 

Sau khi xử lý, chúng tôi lấy tối đa trên cực tiểu {5, 2}, cho kết quả 5. 

Dấu vết này cho thấy các bản cập nhật lặp lại chỉ tinh chỉnh mức tối thiểu được lưu trữ và thứ tự không ảnh hưởng đến tính chính xác. 

### Ví dụ 2 

đầu vào:```
1
3
1 10
2 3
3 7
```| Bước | Nhà hàng | Giá trị | Trạng thái lưu trữ | 
| --- | --- | --- | --- | 
| 1 | 1 | 10 | {1: 10} | 
| 2 | 2 | 3 | {1: 10, 2: 3} | 
| 3 | 3 | 7 | {1: 10, 2: 3, 3: 7} | 

Cực tiểu cuối cùng là {10, 3, 7} nên đáp án là 10. 

Trường hợp này xác nhận rằng khi mỗi nhà hàng có một món ăn duy nhất, giải pháp sẽ giảm chính xác đến giá trị tối đa đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi món ăn được xử lý một lần và mỗi thao tác từ điển là trung bình O(1) | 
| Không gian | O(k) | k là số lượng nhà hàng riêng biệt được lưu trong từ điển | 

Tổng n trên các trường hợp thử nghiệm tối đa là 5 × 10^5, do đó, một giải pháp tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ được giới hạn bằng cách lưu trữ tối đa một mục nhập cho mỗi nhà hàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        best = {}
        for _ in range(n):
            a, d = map(int, input().split())
            if a in best:
                best[a] = min(best[a], d)
            else:
                best[a] = d

        ans = max(best.values())
        out.append(str(ans))

    return "\n".join(out)

# provided sample-style test
assert run("""1
4
4 5
10 7
4 6
10 2
""") == "5"

# minimum size
assert run("""1
1
1 100
""") == "100"

# all same restaurant
assert run("""1
3
5 10
5 3
5 7
""") == "3"

# multiple restaurants, mixed order
assert run("""1
6
1 8
2 6
1 4
3 9
2 1
3 7
""") == "7"

# all unique restaurants
assert run("""1
4
1 5
2 4
3 3
4 2
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| món ăn đơn | 100 | trường hợp ranh giới tối thiểu | 
| cùng một nhà hàng lặp đi lặp lại | 3 | theo dõi tối thiểu chính xác | 
| cập nhật hỗn hợp | 7 | trật tự độc lập | 
| tất cả đều độc đáo | 5 | max trên các nhóm phần tử đơn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một nhà hàng xuất hiện nhiều lần với giá trị giảm dần theo thời gian. Ví dụ: đầu vào:```
1
4
1 10
1 5
1 7
1 3
```Thuật toán cập nhật giá trị được lưu trữ từng bước: 

đầu tiên là 10, sau đó là 5, sau đó giữ nguyên là 5 sau 7, rồi trở thành 3. Bất biến mà giá trị được lưu trữ luôn là giá trị tối thiểu được thấy cho đến nay đảm bảo tính chính xác và đầu ra cuối cùng là 3. 

Một trường hợp khác là khi nhiều nhà hàng cạnh tranh chặt chẽ:```
1
5
1 10
2 9
3 8
4 7
5 6
```Mỗi nhà hàng có một giá trị duy nhất nên từ điển lưu trữ tất cả các giá trị không thay đổi. Giá trị tối đa cuối cùng trả về chính xác là 10, cho thấy thuật toán giảm xuống mức lựa chọn tối đa đơn giản khi không cần sàng lọc nhóm.
