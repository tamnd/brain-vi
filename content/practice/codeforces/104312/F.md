---
title: "CF 104312F - Ngọc Rồng"
description: "Chúng tôi đang duy trì một bộ sưu tập động gồm các "môi trường sống", trong đó mỗi môi trường sống lưu trữ nhiều con rồng được đặt tên và mỗi con rồng có một giá trị kích thước duy nhất. Hệ thống hỗ trợ hai hoạt động theo thời gian. Một hoạt động sẽ đưa một con rồng mới vào môi trường sống đã chọn."
date: "2026-07-01T19:53:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "F"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 67
verified: true
draft: false
---

[CF 104312F - Ngọc rồng](https://codeforces.com/problemset/problem/104312/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập động gồm các "môi trường sống", trong đó mỗi môi trường sống lưu trữ nhiều con rồng được đặt tên và mỗi con rồng có một giá trị kích thước duy nhất. Hệ thống hỗ trợ hai hoạt động theo thời gian. Một hoạt động sẽ đưa một con rồng mới vào môi trường sống đã chọn. Hoạt động còn lại yêu cầu, đối với một môi trường sống nhất định, con rồng nào hiện có kích thước nhỏ nhất và con nào có kích thước lớn nhất, trả lại tên của chúng. 

Chi tiết quan trọng là môi trường sống phát triển dần dần. Mỗi truy vấn phản ánh trạng thái sau tất cả các lần chèn trước đó. Không có sự xóa bỏ nên bộ rồng ở mỗi môi trường sống chỉ phát triển. 

Mặc dù kích thước nhỏ, nhiều nhất là 100, nhưng số lượng thao tác có thể lớn. Điều đó ngay lập tức gợi ý rằng việc quét liên tục tất cả những con rồng trong một môi trường sống cho mỗi truy vấn sẽ trở nên tốn kém nếu có nhiều con rồng tích tụ ở một nơi. Trường hợp xấu nhất là đưa hàng nghìn con rồng vào một môi trường sống rồi liên tục yêu cầu mức tối thiểu và tối đa, điều này sẽ buộc phải quét toàn bộ mỗi lần. 

Một cách tiếp cận ngây thơ sẽ xây dựng lại hoặc quét lại toàn bộ môi trường sống theo mọi yêu cầu. Điều này không thành công khi cùng một môi trường sống tích tụ nhiều con rồng, vì mỗi truy vấn sẽ trở thành tuyến tính ở kích thước hiện tại của nó, dẫn đến hành vi bậc hai nói chung. 

Các trường hợp biên quan trọng ở đây chủ yếu là về các truy vấn lặp lại và phân phối sai lệch. Ví dụ: nếu tất cả các hoạt động nhắm vào một môi trường sống duy nhất: 

đầu vào:```
5
add A a 1
add A b 2
add A c 3
ask A
ask A
```Một giải pháp đơn giản có thể tính lại giá trị tối thiểu và tối đa mỗi lần bằng cách quét tất cả các con rồng được lưu trữ, lặp lại cùng một công việc hai lần. Điều đó vẫn đúng nhưng không hiệu quả ở quy mô lớn. 

Một trường hợp tế nhị khác là khi kích thước cực kỳ nhỏ nhưng tên lại xác định danh tính. Vì tên là duy nhất nên chúng ta không thể dựa vào việc sắp xếp theo tên hoặc giả định tính ổn định trong thứ tự chèn; chỉ có kích thước xác định thứ tự. 

## Phương pháp tiếp cận 

Đối với mỗi môi trường sống, chiến lược vũ phu lưu trữ danh sách tất cả những con rồng khi chúng đến. Đối với một`add`, chúng tôi nối thêm một bản ghi. Đối với một`ask`, chúng tôi quét toàn bộ danh sách để tìm kích thước tối thiểu và tối đa. Điều này đúng vì mọi truy vấn đều tính toán lại thứ tự từ đầu một cách rõ ràng, đảm bảo chúng tôi luôn trả về các giá trị cực trị cập nhật. 

Tuy nhiên, mỗi`ask`chi phí O(k), trong đó k là số lượng rồng trong môi trường sống đó tại thời điểm đó. Nếu tất cả các hoạt động diễn ra trên một môi trường sống và một nửa trong số đó là truy vấn thì tổng chi phí sẽ trở thành O(n^2) trong trường hợp xấu nhất, quá chậm khi n lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần có trật tự đầy đủ, chỉ có hai phần tử cực trị cho mỗi môi trường sống. Điều đó cho thấy việc duy trì những thái cực này dần dần. Thay vì tính toán lại chúng, chúng tôi theo dõi mức tối thiểu và tối đa hiện tại khi chúng tôi chèn các phần tử. Mỗi lần chèn cập nhật tối đa hai phép so sánh. Điều này làm giảm mọi hoạt động xuống O(1). 

Chúng ta cũng cần bảo tồn những tên rồng gắn liền với những thái cực này, vì vậy trạng thái được lưu trữ trên mỗi môi trường sống phải giữ cả kích thước và tên của những ứng cử viên tốt nhất hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một từ điển được khóa theo tên môi trường sống. Mỗi giá trị lưu trữ hai bản ghi: con rồng tối thiểu hiện tại (kích thước và tên) và con rồng tối đa hiện tại. 

1. Khởi tạo một bản đồ trống từ tên môi trường sống đến cấu trúc có giá trị tối thiểu và tối đa là không xác định. 

Điều này đảm bảo môi trường sống được tạo ra một cách dễ dàng ngay lần sử dụng đầu tiên. 
2. Đối với mỗi`add`hoạt động, trích xuất tên môi trường sống, tên rồng và kích thước. 

Nếu môi trường sống không tồn tại, hãy khởi tạo cả giá trị tối thiểu và tối đa cho con rồng này. 
3. Nếu môi trường sống đã tồn tại, hãy so sánh kích thước rồng mới với kích thước tối thiểu được lưu trữ. Nếu nó nhỏ hơn, hãy cập nhật bản ghi tối thiểu. 

Điều này bảo toàn tính bất biến rằng mức tối thiểu được lưu trữ luôn là mức nhỏ nhất được thấy cho đến nay. 
4. So sánh kích thước rồng mới với kích thước tối đa được lưu trữ. Nếu nó lớn hơn, hãy cập nhật bản ghi tối đa. 

Điều này đảm bảo mức tối đa vẫn chính xác sau mỗi lần chèn. 
5. Đối với mỗi`ask`hoạt động, xuất trực tiếp tên tối thiểu và tên tối đa được lưu trữ. 

Không cần quét vì cấu trúc luôn được cập nhật. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mỗi môi trường sống lưu trữ chính xác hai giá trị tóm tắt tất cả những con rồng được thấy cho đến nay: nhỏ nhất theo kích thước và lớn nhất theo kích thước. Mỗi lần chèn được xử lý chính xác một lần và bất cứ khi nào một con rồng mới có thể ảnh hưởng đến một trong hai thái cực, chúng tôi sẽ cập nhật nó ngay lập tức. Vì không có thao tác nào loại bỏ rồng hoặc sửa đổi kích thước nên khi giá trị trở thành tối thiểu hoặc tối đa, nó sẽ giữ nguyên như vậy trừ khi giá trị cực đoan hơn xuất hiện sau đó. Điều này đảm bảo rằng cặp được lưu trữ luôn chính xác khi được truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    habitats = {}

    for _ in range(t):
        parts = input().split()
        if parts[0] == "add":
            _, h, name, size = parts
            size = int(size)

            if h not in habitats:
                habitats[h] = {
                    "min_name": name,
                    "min_size": size,
                    "max_name": name,
                    "max_size": size
                }
            else:
                cur = habitats[h]

                if size < cur["min_size"]:
                    cur["min_size"] = size
                    cur["min_name"] = name

                if size > cur["max_size"]:
                    cur["max_size"] = size
                    cur["max_name"] = name

        else:
            _, h = parts
            cur = habitats[h]
            sys.stdout.write(cur["min_name"] + " " + cur["max_name"] + "\n")

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng từ điển để nhóm tất cả trạng thái theo tên môi trường sống. Mỗi môi trường sống chỉ lưu trữ hai con rồng ứng cử viên, do đó việc sử dụng bộ nhớ vẫn tuyến tính theo số lần chèn. 

Logic cập nhật cẩn thận để so sánh kích thước một cách độc lập ở mức tối thiểu và tối đa. Sự phân tách này rất quan trọng vì cùng một con rồng có thể đồng thời trở thành cả mức tối thiểu và tối đa khi nó lần đầu tiên được đưa vào môi trường sống. 

Đầu ra được viết bằng cách sử dụng`sys.stdout.write`để tránh chi phí từ các cuộc gọi in lặp đi lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
add A x 5
add A y 2
add A z 8
ask A
ask A
```| Bước | Hoạt động | Tối thiểu | Tối đa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | cộng A x 5 | x(5) | x(5) | | 
| 2 | cộng A y 2 | y(2) | x(5) | | 
| 3 | thêm A z 8 | y(2) | z(8) | | 
| 4 | hỏi A | y | z | yz | 
| 5 | hỏi A | y | z | y z | 

Điều này xác nhận rằng các truy vấn lặp lại không tính toán lại trạng thái mà sử dụng lại cực trị được duy trì. 

### Ví dụ 2 

đầu vào:```
add B a 10
add B b 1
add B c 50
add B d 1
ask B
```| Bước | Hoạt động | Tối thiểu | Tối đa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | thêm B a 10 | một(10) | một(10) | | 
| 2 | thêm B b 1 | b(1) | một(10) | | 
| 3 | cộng B c 50 | b(1) | c(50) | | 
| 4 | thêm B d 1 | b(1) | c(50) | | 
| 5 | hỏi B | b | c | b c | 

Điều này chứng tỏ rằng các dây buộc ở kích thước tối thiểu được xử lý một cách tự nhiên bằng cách giữ nguyên mức cực đoan được quan sát đầu tiên; không cần phải bẻ dây buộc đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi hoạt động cập nhật hoặc đọc trạng thái thời gian không đổi trên mỗi môi trường sống | 
| Không gian | O(h) | Một kỷ lục cho mỗi môi trường sống lưu trữ hai con rồng | 

Các ràng buộc cho phép tối đa 100 thao tác, nhưng ngay cả khi được mở rộng quy mô cao hơn, giải pháp này vẫn tuyến tính và hiệu quả. Cập nhật liên tục đảm bảo không bị tắc nghẽn ngay cả trong các hoạt động dày đặc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys

    input = sys.stdin.readline
    t = int(input())
    habitats = {}
    out = []

    for _ in range(t):
        parts = input().split()
        if parts[0] == "add":
            _, h, name, size = parts
            size = int(size)

            if h not in habitats:
                habitats[h] = {
                    "min_name": name,
                    "min_size": size,
                    "max_name": name,
                    "max_size": size
                }
            else:
                cur = habitats[h]
                if size < cur["min_size"]:
                    cur["min_size"] = size
                    cur["min_name"] = name
                if size > cur["max_size"]:
                    cur["max_size"] = size
                    cur["max_name"] = name
        else:
            _, h = parts
            cur = habitats[h]
            out.append(cur["min_name"] + " " + cur["max_name"])

    return "\n".join(out)

# provided sample
assert run("""9
add garden saladmander 5
add garden leekachu 6
add mountain coldasaur 8
ask garden
ask mountain
add garden myrtle 2
add lake fishy 3
ask garden
ask lake
""") == """saladmander leekachu
coldasaur coldasaur
myrtle leekachu
fishy fishy"""

# single element habitat
assert run("""2
add a dragon 10
ask a
""") == "dragon dragon"

# strictly increasing
assert run("""4
add a a 1
add a b 2
add a c 3
ask a
""") == "a c"

# alternating updates
assert run("""6
add a x 5
add a y 1
add a z 10
add a w 0
ask a
ask a
""") == """w z
w z"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thêm một lần rồi hỏi | rồng rồng | khởi tạo một phần tử | 
| tăng kích thước | một c | độ chính xác theo dõi tối đa | 
| xen kẽ các thái cực | w z | cập nhật lặp đi lặp lại cho cả hai đầu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi con rồng đầu tiên trong môi trường sống cũng ở mức tối thiểu và tối đa. Thuật toán khởi tạo cả hai trường cho con rồng đó nên trạng thái nhất quán ngay lập tức. 

đầu vào:```
2
add x a 7
ask x
```Việc thực thi đặt cả giá trị tối thiểu và tối đa thành (a, 7), sau đó truy vấn sẽ đọc chúng trực tiếp, tạo ra`a a`. 

Một trường hợp khác là khi một con rồng mới trở thành mức tối thiểu hoặc tối đa mới riêng biệt theo thời gian. 

đầu vào:```
4
add h a 5
add h b 1
add h c 10
ask h
```Sau bước 2, b trở thành min. Sau bước 3, c trở thành tối đa. Cấu trúc theo dõi chính xác cả hai độc lập, vì vậy câu trả lời cuối cùng là`b c`. 

Trường hợp tế nhị cuối cùng là lặp lại những câu hỏi mà không can thiệp thêm. Vì không xảy ra tính toán lại trong quá trình truy vấn nên các kết quả đầu ra lặp lại vẫn giống hệt nhau, phù hợp với yêu cầu trạng thái không thay đổi trong các hoạt động truy vấn.
