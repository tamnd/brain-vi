---
title: "CF 104636H - Chủ khách sạn"
description: "Chúng tôi đang mô phỏng một khách sạn rất nhỏ với chính xác mười phòng được đánh số từ 0 đến 9. Mỗi phòng có thể trống hoặc chỉ có một khách ở. Theo thời gian, khách đến từ một trong hai lối vào hoặc rời khỏi một phòng cụ thể."
date: "2026-06-29T17:07:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "H"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 64
verified: true
draft: false
---

[CF 104636H - Chủ khách sạn](https://codeforces.com/problemset/problem/104636/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một khách sạn rất nhỏ với chính xác mười phòng được đánh số từ 0 đến 9. Mỗi phòng có thể trống hoặc chỉ có một khách ở. Theo thời gian, khách đến từ một trong hai lối vào hoặc rời khỏi một phòng cụ thể. 

Hành vi khi đến là quy tắc quan trọng: nếu khách vào từ lối vào bên trái, họ luôn chiếm phòng ngoài cùng bên trái hiện đang trống. Nếu họ vào từ lối vào bên phải, họ luôn chiếm căn phòng trống hiện tại ngoài cùng bên phải. Việc khởi hành rất rõ ràng: khi chúng ta nhìn thấy chữ số x, khách hiện đang ở phòng x sẽ rời đi, khiến phòng lại trống. 

Đầu vào là trình tự thời gian của các sự kiện này. Chúng ta phải mô phỏng chúng theo thứ tự và duy trì trạng thái lấp đầy của mười phòng. Đầu ra cuối cùng là một chuỗi nhị phân gồm 10 ký tự, trong đó mỗi vị trí cho biết phòng đó có được sử dụng ở cuối hay không. 

Các ràng buộc rất lớn về số lượng sự kiện, lên tới 100.000 hoạt động. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào cũng phải xử lý từng sự kiện trong thời gian không đổi. Vì không gian trạng thái rất nhỏ (chỉ 10 phòng), nên chúng tôi không bị hạn chế bởi bộ nhớ hoặc kích thước cấu trúc, chỉ bằng cách tránh công việc quét lặp lại cho mỗi sự kiện sẽ nhân lên thành khoảng 10 × 100.000, điều này vẫn ổn, nhưng mọi thứ phức tạp hơn hoặc gián tiếp hơn sẽ không cần thiết. 

Một sai lầm ngây thơ thường xuất phát từ việc cố gắng xây dựng lại các nhiệm vụ hoặc theo dõi động “ai ở vị trí nào” bằng cấu trúc dữ liệu phức tạp hoặc tính toán lại các phòng trống gần nhất bằng cách sử dụng tính năng quét toàn bộ cho mọi sự kiện mà không có cấu trúc cẩn thận. Mặc dù quét 10 vị trí liên tục, việc lặp lại logic không cần thiết hoặc quản lý cập nhật trạng thái sai khi khởi hành có thể gây ra kết quả không chính xác. 

Một trường hợp tinh tế là sử dụng lại các phòng sau khi rời đi. Ví dụ: nếu một phòng được giải phóng và sau đó một người đến khác sẽ sử dụng lại phòng đó dựa trên các quy tắc về khoảng cách, thì thuật toán phải xem xét lại một cách chính xác rằng phòng đó có sẵn ngay lập tức. 

Một cạm bẫy tiềm tàng khác là hiểu lầm rằng việc khởi hành không phụ thuộc vào thứ tự đến mà phụ thuộc trực tiếp vào chỉ số phòng. Một cách tiếp cận sai lầm có thể cố gắng theo dõi khách xếp hàng ở mỗi lối vào, nhưng điều đó là không cần thiết và dễ xảy ra lỗi. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản hoạt động được vì hệ thống cực kỳ nhỏ. Chúng tôi duy trì một mảng có kích thước 10 cho biết mỗi phòng có người sử dụng hay không. Với mỗi lần đến, chúng tôi quét từ trái sang phải hoặc phải sang trái để tìm ô trống đầu tiên và chỉ định. Đối với mỗi lần khởi hành, chúng tôi chỉ cần đánh dấu phòng trống. 

Cách tiếp cận mạnh mẽ này đã tối ưu trong thực tế vì việc quét 10 phần tử là thời gian không đổi. Ngay cả với 100.000 thao tác, chúng tôi thực hiện tối đa khoảng 1.000.000 lần kiểm tra nguyên thủy, điều này không đáng kể. 

Chúng tôi có thể cố gắng “tối ưu hóa” bằng cách duy trì các cấu trúc riêng biệt cho các phòng trống gần nhất từ ​​mỗi bên, nhưng điều đó sẽ làm tăng thêm sự phức tạp mà không mang lại lợi ích gì, vì việc tính toán lại trên 10 vị trí vốn đã không đáng kể. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng không gian trạng thái là cố định và rất nhỏ, vì vậy mô phỏng trực tiếp là giải pháp mong muốn. Vấn đề được thiết kế để kiểm tra việc triển khai cẩn thận hơn là tối ưu hóa thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(10n) | O(1) | Đã chấp nhận | 
| Mô phỏng trực tiếp tối ưu | O(10n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một danh sách`rooms`có kích thước 10, ban đầu tất cả đều là số không. 

### Các bước 

1. Khởi tạo một mảng`rooms`có độ dài 10 với tất cả các giá trị được đặt thành 0. 

Điều này thể hiện rằng tất cả các phòng đều trống khi bắt đầu. 
2. Xử lý từng ký tự trong chuỗi đầu vào theo thứ tự. 

Tính đúng đắn phụ thuộc vào việc bảo toàn chính xác thứ tự thời gian. 
3. Nếu nhân vật là`'L'`, quét từ phòng 0 trở lên cho đến khi tìm thấy phòng trống đầu tiên. 

Chỉ định nó là bị chiếm dụng và dừng lại ngay lập tức. 

Lý do điều này có tác dụng là vì quy tắc ưu tiên rõ ràng vị trí có sẵn ngoài cùng bên trái. 
4. Nếu nhân vật là`'R'`, quét từ phòng 9 trở xuống cho đến khi tìm được phòng trống đầu tiên. 

Chỉ định nó là bị chiếm dụng và dừng lại ngay lập tức. 

Điều này thực thi quy tắc lựa chọn ngoài cùng bên phải đối xứng. 
5. Nếu ký tự là một chữ số từ`'0'`ĐẾN`'9'`, chuyển đổi nó thành số nguyên x và đánh dấu`rooms[x] = 0`. 

Điều này trực tiếp mô hình hóa việc khởi hành, giải phóng phòng được chỉ định bất kể trạng thái trước đó. 
6. Sau khi xử lý tất cả các sự kiện, xuất mảng dưới dạng chuỗi 0 và 1. 

### Tại sao nó hoạt động 

Ở mỗi bước,`rooms[i]`phản ánh chính xác liệu phòng`i`bị chiếm dụng sau khi xử lý tất cả các sự kiện trước đó. Người đến luôn chọn phòng trống đầu tiên theo hướng yêu cầu và người khởi hành chỉ dọn một chỗ cụ thể mà không ảnh hưởng đến người khác. Vì không có sự kiện nào phụ thuộc vào bất cứ điều gì ngoại trừ trạng thái chiếm chỗ hiện tại nên việc duy trì bất biến này đảm bảo tính chính xác cho cấu hình cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    s = input().strip()

    rooms = [0] * 10

    for c in s:
        if c == 'L':
            for i in range(10):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        elif c == 'R':
            for i in range(9, -1, -1):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        else:
            idx = ord(c) - ord('0')
            rooms[idx] = 0

    print(''.join(map(str, rooms)))

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp theo mô hình mô phỏng. Phần tinh tế duy nhất là xử lý các lượt đến bằng cách quét tuyến tính theo đúng hướng. Vì kích thước mảng được cố định ở mức 10 nên thời gian này vẫn không đổi cho mỗi sự kiện. 

Việc xử lý chữ số dựa trên chuyển đổi ASCII, giúp tránh mọi chi phí phân tích cú pháp và đảm bảo mỗi lần khởi hành sẽ ánh xạ trực tiếp tới một chỉ mục phòng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
LLRL1RL1
```Chúng tôi theo dõi trạng thái phòng: 

| Bước | Sự kiện | Trạng thái phòng | 
| --- | --- | --- | 
| 0 | ban đầu | 0000000000 | 
| 1 | L | 1000000000 | 
| 2 | L | 1100000000 | 
| 3 | R | 1100000001 | 
| 4 | L | 1110000001 | 
| 5 | 1 | 1010000001 | 
| 6 | R | 1010000011 | 
| 7 | L | 1110000011 | 
| 8 | 1 | 1010000011 | 

Đầu ra cuối cùng là`1010000011`. 

Dấu vết này xác nhận rằng những người đến luôn chọn những phòng trống nhất trong ranh giới, đồng thời việc xóa ngay lập tức mở ra cơ hội sử dụng lại cho những người đến sau. 

### Ví dụ 2 

đầu vào:```
9
L0L0LLRR9
```| Bước | Sự kiện | Trạng thái phòng | 
| --- | --- | --- | 
| 0 | ban đầu | 0000000000 | 
| 1 | L | 1000000000 | 
| 2 | 0 | 0000000000 | 
| 3 | L | 1000000000 | 
| 4 | 0 | 0000000000 | 
| 5 | L | 1000000000 | 
| 6 | L | 1100000000 | 
| 7 | R | 1100000001 | 
| 8 | R | 1100000011 | 
| 9 | 9 | 1100000010 | 

Đầu ra cuối cùng là`1100000010`. 

Điều này chứng tỏ việc sử dụng lại cùng một phòng nhiều lần sau khi khởi hành, cho thấy hệ thống không theo dõi danh tính của khách mà chỉ theo dõi tình trạng sử dụng phòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10n) | Mỗi sự kiện kích hoạt quét tối đa 10 phòng | 
| Không gian | O(1) | Chỉ duy trì một mảng có kích thước cố định gồm 10 phòng | 

Với n lên tới 100.000, tổng số hoạt động vẫn còn khoảng một triệu kiểm tra đơn giản, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    rooms = [0] * 10

    for c in s:
        if c == 'L':
            for i in range(10):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        elif c == 'R':
            for i in range(9, -1, -1):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        else:
            rooms[ord(c) - 48] = 0

    return ''.join(map(str, rooms))

# provided samples
assert solve("8\nLLRL1RL1\n") == "1010000011"
assert solve("9\nL0L0LLRR9\n") == "1100000010"

# custom cases
assert solve("1\nL\n") == "1000000000"
assert solve("2\nLR\n") == "1000000001"
assert solve("10\nLLLLLLLLLL\n") == "1111111111"
assert solve("20\nLRLRLRLRLRLRLRLRLRLR\n") == "1111111111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 L | 1000000000 | đơn trái đến | 
| LR | 1000000001 | tái sử dụng sau khi đến nơi | 
| tất cả L | 1111111111 | kín phòng | 
| xen kẽ LR | 1111111111 | ổn định khi có lượng khách đến hỗn hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là lặp đi lặp lại việc lấp đầy và dọn trống cùng một phòng. Xem xét đầu vào`L0L`. Sau lần đến đầu tiên, phòng 0 đã được lấp đầy. chữ số`0`giải phóng nó một lần nữa, và cuối cùng`L`phải sử dụng lại chính xác phòng 0 làm vị trí trống ngoài cùng bên trái. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần đến luôn quét từ đầu mảng và vị trí giải phóng sẽ hiển thị ngay lập tức. 

Một trường hợp khác là lượng khách đến xen kẽ liên tục đảo ngược tình trạng chiếm chỗ ở cả hai đầu, chẳng hạn như`LRLRLR...`. Hệ thống không bao giờ được “bỏ qua” một phòng trống do trạng thái cũ. Vì mỗi bước tính toán lại tình trạng sẵn có trực tiếp từ`rooms`, không có giả định lịch sử nào được đưa ra nên việc sử dụng lại luôn phản ánh cấu hình hiện tại. 

Trường hợp thứ ba là hết chỗ, sau đó là khởi hành. Ngay cả khi tất cả các phòng đã đầy, một sự kiện chữ số sẽ ngay lập tức giải phóng một phòng và người đến tiếp theo phải chọn chính xác dựa trên trạng thái cập nhật. Vì mọi thao tác đều cập nhật cùng một mảng chia sẻ nên không có trạng thái ẩn nào có thể trì hoãn việc cập nhật tính khả dụng.
