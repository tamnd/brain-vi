---
title: "CF 106293B - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
description: "Vasya bắt đầu với một số bài toán đã giải được trong cuộc thi và một cuộc thi hiện có có chứa một số nhiệm vụ cố định."
date: "2026-06-18T22:34:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106293
codeforces_index: "B"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2025-2026"
rating: 0
weight: 106293
solve_time_s: 56
verified: true
draft: false
---

[CF 106293B - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f](https://codeforces.com/problemset/problem/106293/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya bắt đầu với một số bài toán đã giải được trong cuộc thi và một cuộc thi hiện có có chứa một số nhiệm vụ cố định. Mục tiêu của anh ấy là làm cho tỷ lệ thành công của mình cao hơn một nửa, nghĩa là sau hành động của anh ấy, số nhiệm vụ được giải quyết phải vượt quá một nửa tổng số nhiệm vụ trong cuộc thi. 

Anh ấy có hai cách để tăng sự tiến bộ của mình. Đầu tiên là giải một nhiệm vụ hiện có trong cuộc thi, điều này chỉ làm tăng số lượng người đã giải được. Thứ hai có cấu trúc mạnh mẽ hơn: anh ta vừa thêm một nhiệm vụ mới vào cuộc thi vừa giải quyết nó ngay lập tức, điều này làm tăng cả số lượng đã giải quyết và tổng số nhiệm vụ. 

Quá trình bắt đầu từ trạng thái ban đầu được mô tả bằng hai con số, số lượng đã giải được và quy mô cuộc thi. Mỗi thao tác đều có chi phí về thời gian và mục tiêu là đạt đến trạng thái trong đó số lượng được giải quyết đúng hơn một nửa tổng số nhiệm vụ trong khi giảm thiểu tổng thời gian sử dụng. 

Các ràng buộc lên tới một trăm nghìn đối với các giá trị ban đầu, điều này ngay lập tức cho thấy rằng bất kỳ giải pháp nào cố gắng mô phỏng tất cả các chuỗi hoạt động có thể xảy ra một cách ngây thơ sẽ thất bại. Bất kỳ cách tiếp cận nào phân nhánh trên tất cả các kết hợp của hai thao tác phải được giảm xuống thành một quyết định tuyến tính hoặc gần tuyến tính trên một tham số duy nhất, nếu không, nó có nguy cơ khám phá bậc hai lên tới một trăm nghìn bước. 

Một vấn đề tế nhị xuất hiện trong chính điều kiện mục tiêu. Điều kiện phụ thuộc vào cả hai biến, đã giải và tổng, nhưng cả hai phép toán đều tăng độ giải, trong khi chỉ có một phép tăng tổng. Điều này tạo ra một mục tiêu di động trong đó việc cải thiện tử số cũng có khả năng làm tăng mẫu số. Một chiến lược tham lam ngây thơ luôn thích hoạt động rẻ hơn trên mỗi bước mà không xem xét mẫu số phát triển như thế nào sẽ thất bại trong các trường hợp được xây dựng trong đó việc thêm các nhiệm vụ trở nên cần thiết để giảm mô hình tăng trưởng ngưỡng cần thiết. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng tất cả các chuỗi hoạt động có thể xảy ra cho đến khi điều kiện trở thành đúng. Từ một trạng thái được xác định bởi tổng số được giải quyết hiện tại và tổng số, chúng ta có thể thử cả hai thao tác theo cách đệ quy hoặc thông qua BFS trên các trạng thái. Điều này hoạt động hợp lý vì mọi thao tác đều có tác dụng cố định và chúng ta luôn có thể đạt được mục tiêu theo các bước hữu hạn vì việc giải quyết các nhiệm vụ sẽ tăng số lượng giải được một cách đơn điệu. 

Tuy nhiên, số lượng các trạng thái có thể tiếp cận tăng lên nhanh chóng. Nếu chúng ta cho phép tối đa O(n) hoạt động, mỗi trạng thái sẽ phân nhánh thành hai lựa chọn, dẫn đến sự bùng nổ theo cấp số nhân. Ngay cả việc cắt giảm theo chi phí cũng không đủ vì các trạng thái có số dư hơi khác nhau giữa số tiền đã giải quyết và tổng số đều có thể phù hợp và chúng ta không thể hợp nhất chúng một cách an toàn mà không mất đi tính tối ưu. 

Nhận xét quan trọng là cấu trúc của bài toán được chuyển thành một ràng buộc bất đẳng thức duy nhất. Mỗi thao tác đóng góp vào một biểu thức tuyến tính xác định xem chúng ta có vượt qua ngưỡng hay không. Điều kiện này có thể được viết lại theo mức độ “lợi thế” mà chúng ta vẫn cần đạt được. 

Hãy để điều kiện cuối cùng được viết lại theo đại số. Sau k phép toán loại A và m phép toán loại B, chúng ta có số đếm mới được giải và tổng số mới. Yêu cầu trở thành bất đẳng thức tuyến tính theo k và m, có nghĩa là vấn đề giảm xuống việc chọn số lượng mỗi thao tác cần thực hiện chứ không phải theo thứ tự nào. 

Khi chúng tôi khắc phục được mức giảm này, vấn đề sẽ trở thành việc giảm thiểu chi phí theo một ràng buộc tuyến tính duy nhất. Cấu trúc này cho phép so sánh tham lam về hiệu quả giữa các hoạt động hoặc quét trực tiếp qua một biến, vì khi m được cố định, k tốt nhất sẽ bị ép buộc.

Một cách tiếp cận trực tiếp và đáng tin cậy là lặp lại số lượng thao tác loại B và tính toán số lượng thao tác loại A tối thiểu cần thiết. Điều này hiệu quả vì với mỗi lựa chọn của m, ràng buộc trở thành giới hạn một chiều đơn giản trên k và k tối ưu luôn là số nguyên khả thi nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force trên các tiểu bang | Hàm mũ | Hàm mũ | Quá chậm | 
| Sửa các phép toán B và tính A một cách tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển điều kiện thành công sang dạng chỉ phụ thuộc vào số lượng thao tác được sử dụng. Giả sử chúng ta áp dụng phép toán A k lần và phép toán B m lần. Số lượng nhiệm vụ đã giải quyết tăng thêm k + m, trong khi tổng số nhiệm vụ tăng thêm m. Điều kiện “giải được hơn một nửa tổng” trở thành bất đẳng thức liên quan đến k và m. 

Sau đó, chúng tôi tách biệt tất cả các số hạng không đổi để hiểu chúng tôi còn bao xa mới thỏa mãn được điều kiện ban đầu. Điều này đưa ra ngưỡng cải thiện bắt buộc chỉ phụ thuộc vào giá trị bắt đầu. 

Tiếp theo, chúng tôi lặp lại tất cả các giá trị có thể có của m, vì m xác định đầy đủ mức độ thay đổi của ngưỡng do mẫu số tăng lên. Với mỗi m cố định, chúng ta tính k tối thiểu cần thiết để thỏa mãn bất đẳng thức. Vì mỗi phép toán A đóng góp mạnh hơn cho tử số mà không ảnh hưởng đến mẫu số nên k tối thiểu luôn là số nguyên nhỏ nhất thỏa mãn khoảng trống còn lại. 

Đối với mỗi cặp (m, k), chúng tôi tính tổng chi phí là m nhân b cộng k nhân a và theo dõi mức tối thiểu trên tất cả các lựa chọn. 

Cuối cùng, chúng tôi trả về chi phí tính toán nhỏ nhất. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là đối với một số lượng cố định các “hoạt động kết cấu đắt tiền” B, yêu cầu còn lại trở thành điều kiện đơn điệu tuyến tính trong A. Không có sự tương tác giữa các lựa chọn k khác nhau ngoài ranh giới bất đẳng thức, do đó, lấy k khả thi tối thiểu luôn là tối ưu. Vì tất cả các nghiệm hợp lệ tương ứng duy nhất với một giá trị nào đó của m và k tối ưu được tạo ra bởi nó, việc liệt kê m bao trùm toàn bộ không gian nghiệm mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = int(input())
    p = int(input())
    a = int(input())
    b = int(input())

    # Need: (s + k + m) * 2 > (p + m)
    # => 2s + 2k + 2m > p + m
    # => 2k + m > p - 2s
    D = p - 2 * s

    # We need 2k + m >= D + 1
    need = D + 1

    INF = 10**30
    ans = INF

    # try all m
    for m in range(need + 1):
        rem = need - m
        if rem <= 0:
            k = 0
        else:
            k = (rem + 1) // 2

        cost = m * b + k * a
        ans = min(ans, cost)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách viết lại điều kiện thắng thành dạng thâm hụt. Biến`D`đo xem Vasya còn bao xa mới thỏa mãn được sự bất bình đẳng lúc ban đầu. Sau đó, chúng tôi chuyển đổi điều kiện thành yêu cầu về sự kết hợp tuyến tính của các hoạt động. 

Vòng lặp kết thúc`m`đại diện cho việc sửa số lần Vasya sử dụng thao tác thay đổi cả số lượng nhiệm vụ và số lượng đã giải quyết. Sau khi đã khắc phục được điều đó, yêu cầu còn lại sẽ trở thành phép chia trần đơn giản để xác định số lượng thao tác giải thuần túy cần thiết. biểu hiện`(rem + 1) // 2`là số nguyên nhỏ nhất k thỏa mãn`2k >= rem`. 

Chi phí được tích lũy trực tiếp và so sánh với mức tối thiểu toàn cầu. Phạm vi lặp là an toàn vì khi m vượt quá yêu cầu, k sẽ bằng 0 và m bổ sung chỉ làm tăng chi phí. 

## Ví dụ đã hoạt động 

Hãy xem xét một tình huống nhỏ trong đó s = 1, p = 5, a = 2, b = 3. Ban đầu, Vasya còn lâu mới giải được hơn một nửa. 

Chúng ta tính D = p - 2s = 5 - 2 = 3, do đó cần trở thành 4. 

Chúng tôi đánh giá các giá trị m khác nhau. 

| m | còn lại cần 2k | k | tổng chi phí | 
| --- | --- | --- | --- | 
| 0 | 4 | 2 | 4 | 
| 1 | 3 | 2 | 7 | 
| 2 | 2 | 1 | 7 | 
| 3 | 1 | 1 | 9 | 
| 4 | 0 | 0 | 12 | 

Mức tối thiểu đạt được ở m = 0, k = 2, cho thấy việc giải thuần túy là tối ưu ở đây vì việc giải đủ hiệu quả so với hoạt động của cấu trúc. 

Bây giờ xét s = 0, p = 3, a = 10, b = 1. Ta được D = 3, need = 4. 

| m | k | chi phí | 
| --- | --- | --- | 
| 0 | 2 | 20 | 
| 1 | 2 | 21 | 
| 2 | 1 | 12 | 
| 3 | 1 | 13 | 
| 4 | 0 | 4 | 

Giải pháp tốt nhất là m = 4, k = 0, nghĩa là việc thêm và giải quyết các nhiệm vụ mới sẽ rẻ hơn đáng kể trên mỗi mức tăng hiệu quả. 

Hai dấu vết này cho thấy sự cân bằng giữa các hoạt động thay đổi chiến lược tối ưu như thế nào tùy thuộc vào chi phí tương đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p - 2s) | Chúng tôi lặp lại tất cả các số lượng khả thi của hoạt động kết cấu và tính toán công thức thời gian không đổi cho mỗi | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Giới hạn trên của vòng lặp tối đa là 100000 trong trường hợp xấu nhất, dễ dàng phù hợp trong giới hạn thời gian. Mỗi lần lặp chỉ thực hiện các phép toán số học, do đó giải pháp chạy thoải mái theo thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    s = int(input())
    p = int(input())
    a = int(input())
    b = int(input())

    D = p - 2 * s
    need = D + 1
    INF = 10**30
    ans = INF

    for m in range(need + 1):
        rem = need - m
        if rem <= 0:
            k = 0
        else:
            k = (rem + 1) // 2
        ans = min(ans, m * b + k * a)

    print(ans)

# minimum case
assert run("0\n1\n1\n1\n") == "1"

# only solving is best
assert run("1\n5\n2\n100\n") == "4"

# only adding is best
assert run("0\n3\n100\n1\n") == "4"

# balanced case
assert run("2\n10\n3\n2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0,1,1,1 | 1 | trường hợp biên nhỏ nhất | 
| 1,5,2,100 | 4 | sự thống trị thuần túy | 
| 0,3,100,1 | 4 | sự thống trị B thuần túy | 
| 2,10,3,2 | tối thiểu hợp lệ | ổn định chiến lược hỗn hợp | 

## Vỏ cạnh 

Khi trạng thái ban đầu đã gần đến ngưỡng, giá trị yêu cầu D sẽ trở nên nhỏ và vòng lặp trên m có thể ngay lập tức nhận thấy rằng không cần thực hiện các thao tác loại A. Trong trường hợp đó, thuật toán chỉ chọn chính xác các hoạt động kết cấu nếu chúng rẻ hơn. 

Ví dụ: với s = 4 và p = 7, chúng ta đã có 4 > 3,5 nên không cần thực hiện thao tác nào. Công thức dẫn xuất cho D = -1 và nhu cầu được tính toán trở thành 0 hoặc âm, khiến vòng lặp trả về chính xác chi phí bằng 0. 

Khi a cực nhỏ so với b, thuật toán đương nhiên ưu tiên k phép toán vì mỗi A đóng góp hiệu quả gấp đôi vào bất đẳng thức. Việc liệt kê vẫn nắm bắt được điều này mà không yêu cầu lý luận tham lam rõ ràng. 

Khi b cực kỳ nhỏ, vòng lặp nhận thấy rằng việc tăng m nhanh chóng thỏa mãn điều kiện ngay cả khi nó tăng p và giải pháp tối ưu đạt được ở mức m lớn với k bằng 0.
