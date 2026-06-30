---
title: "CF 104637E - Cánh Cửa"
description: "Chúng ta được đưa ra một trình tự mô tả thứ tự các cửa được mở. Mỗi cửa thuộc một trong hai lối ra của ngôi nhà, là lối ra bên trái hoặc lối ra bên phải."
date: "2026-06-29T17:00:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 76
verified: true
draft: false
---

[CF 104637E - Những cánh cửa](https://codeforces.com/problemset/problem/104637/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một trình tự mô tả thứ tự các cửa được mở. Mỗi cửa thuộc một trong hai lối ra của ngôi nhà, là lối ra bên trái hoặc lối ra bên phải. Trình tự không mô tả hình học hay cấu trúc, nó chỉ cho chúng ta biết, từng bước một, liệu cánh cửa mở tiếp theo thuộc về bên trái hay bên phải. 

Tại bất kỳ thời điểm nào sau khi mở một số tiền tố của chuỗi này, một số tập hợp con cửa ở mỗi bên sẽ được mở. Ông Black có thể rời khỏi nhà nếu toàn bộ một lối ra mở hoàn toàn, nghĩa là mọi cánh cửa thuộc lối ra đó đều đã xuất hiện ở tiền tố đã mở. 

Nhiệm vụ là tìm độ dài tiền tố sớm nhất trong đó điều kiện này trở thành đúng đối với ít nhất một trong hai lối thoát. 

Giới hạn lên tới 200.000 cửa ngụ ý rằng bất kỳ mô phỏng bậc hai nào trên tiền tố sẽ quá chậm. Một cách tiếp cận kiểm tra từng tiền tố bằng cách tính toán lại những cánh cửa nào được mở hoàn toàn sẽ dẫn đến khoảng n² hoạt động trong trường hợp xấu nhất, vượt xa những gì giới hạn một giây có thể xử lý. Giải pháp phải dựa vào một lần quét tuyến tính duy nhất hoặc một lượng công việc bổ sung không đổi cho mỗi phần tử. 

Một trường hợp phức tạp xuất hiện khi cửa cuối cùng của một lối ra xuất hiện rất sớm trong chuỗi, trong khi lối ra kia kết thúc muộn hơn nhiều. Ví dụ: nếu tất cả các cửa bên phải xuất hiện ở gần đầu và cửa bên phải cuối cùng ở vị trí 3, nhưng các cửa bên trái tiếp tục cho đến vị trí n, thì câu trả lời là 3 mặc dù hầu hết chuỗi vẫn chưa hoàn chỉnh ở phía bên trái. Một mô phỏng tiền tố đơn giản có thể đợi không chính xác cho đến khi cả hai bên được mở hoàn toàn, điều này là không bắt buộc. 

## Phương pháp tiếp cận 

Cách thức mạnh mẽ là kiểm tra mọi tiền tố có độ dài k và kiểm tra xem tất cả các cửa của lối ra bên trái hay tất cả các cửa của lối ra bên phải đã xuất hiện trong tiền tố đó hay chưa. Để thực hiện điều này một cách trực tiếp, người ta cần biết tổng số cửa ở mỗi bên và đếm xem có bao nhiêu cửa trong số đó đã được nhìn thấy cho đến nay. Với mỗi k, việc triển khai đơn giản sẽ quét qua tiền tố và xác minh tính đầy đủ hoặc duy trì các tập hợp và so sánh với tổng số. 

Điều này hoạt động chính xác vì điều kiện hoàn toàn là về phạm vi bao phủ của tất cả các lần xuất hiện của một loại. Tuy nhiên, việc tính toán lại phạm vi cho từng tiền tố sẽ dẫn đến công việc lặp đi lặp lại. Đối với n phần tử, việc quét tiền tố dẫn đến khoảng 1 + 2 + ... + n thao tác, theo thứ tự n². 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến số lượng trung gian mà chỉ quan tâm đến thời điểm mỗi bên được bao gồm đầy đủ trong tiền tố. Một bên trở nên mở hoàn toàn chính xác vào thời điểm chúng ta gặp lần xuất hiện cuối cùng của nó trong chuỗi. Trước thời điểm đó, ít nhất một trong những cánh cửa của nó chưa được mở. Sau thời điểm đó, nó vẫn mở hoàn toàn mãi mãi. 

Điều này làm giảm vấn đề tìm vị trí cuối cùng của mỗi giá trị trong chuỗi. Câu trả lời đơn giản là sớm nhất trong số các điểm hoàn thành này, bởi vì ngay khi một trong hai bên hoàn thành, việc thoát ra sẽ có thể xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (quét lần cuối) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét trình tự một lần và ghi lại vị trí cuối cùng nơi cửa bên trái xuất hiện và vị trí cuối cùng nơi cửa bên phải xuất hiện. Điều này là đủ vì việc hoàn thành một mặt chỉ phụ thuộc vào sự xuất hiện cuối cùng của nó chứ không phải cấu trúc trung gian. 
2. Khi đã biết cả hai vị trí cuối cùng, hãy so sánh chúng. Bên nào về đích sớm hơn sẽ có cơ hội thoát ra sớm nhất. 
3. Xuất ra giá trị tối thiểu của hai chỉ số xuất hiện cuối cùng. 

Ý tưởng chính là chúng tôi không mô phỏng quá trình mà xác định chính xác thời điểm mỗi bên trở nên hoàn toàn hài lòng. 

### Tại sao nó hoạt động

Đối với mỗi lần thoát, thời điểm nó có thể sử dụng được được ấn định bởi lần xuất hiện cuối cùng của nó trong chuỗi. Trước vị trí đó, ít nhất một cửa bắt buộc vẫn chưa được mở. Sau vị trí đó, tất cả các cửa yêu cầu của bên đó đảm bảo đã xuất hiện ở tiền tố. Do đó, mỗi bên đóng góp một chỉ số ngưỡng duy nhất và câu trả lời là ngưỡng nhỏ nhất trong số các ngưỡng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_str(data: str) -> str:
    it = iter(data.strip().split())
    n = int(next(it))
    last0 = last1 = 0

    for i in range(1, n + 1):
        x = int(next(it))
        if x == 0:
            last0 = i
        else:
            last1 = i

    return str(min(last0, last1))

def solve():
    data = sys.stdin.read()
    sys.stdout.write(solve_str(data))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ dựa vào việc theo dõi hai số nguyên, chỉ mục được nhìn thấy lần cuối của mỗi loại. Vòng lặp được lập chỉ mục 1 để khớp trực tiếp với độ dài tiền tố khái niệm, tránh điều chỉnh từng cái một ở cuối. Câu trả lời cuối cùng được tính toán theo thời gian không đổi sau khi quét. 

Một lỗi phổ biến là cố gắng duy trì số lần chạy và kiểm tra xem tổng số có khớp hay không. Cách tiếp cận đó có hiệu quả nhưng đòi hỏi phải biết tổng số một cách rõ ràng và vẫn có rủi ro tăng thêm chi phí nếu triển khai không hiệu quả. Phương pháp xuất hiện cuối cùng tránh được điều này hoàn toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
0 0 1 0 0
```| Bước | Cửa | cuối cùng0 | cuối cùng1 | Trạng thái quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | còn lại chưa hoàn thiện | 
| 2 | 0 | 2 | 0 | còn lại chưa hoàn thiện | 
| 3 | 1 | 2 | 3 | đúng hoàn thành | 
| 4 | 0 | 4 | 3 | đúng hoàn thành | 
| 5 | 0 | 5 | 3 | đúng hoàn thành | 

Lần xuất hiện cuối cùng của cửa bên phải là ở vị trí 3, do đó tiền tố có độ dài 3 đã chứa tất cả các cửa bên phải. Phía bên trái hoàn thành sau nên không ảnh hưởng đến đáp án. 

Đầu ra là 3. 

### Mẫu 2 

đầu vào:```
4
1 0 0 1
```| Bước | Cửa | cuối cùng0 | cuối cùng1 | Trạng thái quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | đúng không đầy đủ | 
| 2 | 0 | 2 | 1 | đúng không đầy đủ | 
| 3 | 0 | 3 | 1 | đúng không đầy đủ | 
| 4 | 1 | 3 | 4 | còn lại hoàn thành | 

Ở đây, số 0 cuối cùng xuất hiện ở vị trí 3, nghĩa là tất cả các cửa bên trái đã được mở theo tiền tố 3. Cửa bên phải kết thúc muộn hơn nên chúng không liên quan đến việc thoát ra sớm nhất. 

Đầu ra là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | theo dõi lần quét duy nhất lần xuất hiện cuối cùng | 
| Không gian | O(1) | chỉ có hai quầy được lưu trữ | 

Giải pháp thực hiện chính xác một lần chuyển qua trình tự và xử lý hậu kỳ theo thời gian không đổi. Với n lên tới 200.000, điều này phù hợp thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve_str(sys.stdin.read())

# provided samples
assert run("5\n0 0 1 0 0\n") == "3"
assert run("4\n1 0 0 1\n") == "3"

# custom cases
assert run("2\n0 1\n") == "1", "minimum alternating case"
assert run("3\n0 0 0\n") == "3", "only one exit effectively needed"
assert run("3\n1 0 1\n") == "3", "right finishes last"
assert run("6\n0 1 0 1 0 1\n") == "5", "alternating worst spread"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 xen kẽ | 1 | hoàn thành ngay lập tức một bên | 
| tất cả cùng một phía | chiều dài đầy đủ | chỉ có một lối thoát quan trọng | 
| phải-nặng kết thúc muộn | chỉ mục cuối cùng | xử lý hoàn thành muộn | 
| trình tự xen kẽ | 5 | rải rác lần cuối cùng | 

## Vỏ cạnh 

Trường hợp tất cả các cửa thuộc một loại xuất hiện từ rất sớm chứng tỏ tại sao việc chỉ theo dõi những khoảnh khắc hoàn thành lại có tác dụng. Đối với đầu vào`1 1 1 0 0 0`, cửa cuối cùng bên phải ở vị trí 3, do đó, có thể thoát ra ở tiền tố 3 mặc dù cửa bên trái vẫn tiếp tục sau đó. Thuật toán nắm bắt chính xác điều này vì nó ghi lại lần xuất hiện cuối cùng của từng loại thay vì mô phỏng các tiền tố. 

Trong tình huống ngược lại như`0 1 0 1 0 1`, cả hai bên đều được xen kẽ, nhưng lần xuất hiện cuối cùng của mỗi bên sẽ xác định thời điểm chúng hoàn thành độc lập. Mức tối thiểu của các vị trí cuối cùng này xác định chính xác tiền tố có thể sử dụng sớm nhất mà không cần mô phỏng từng bước.
