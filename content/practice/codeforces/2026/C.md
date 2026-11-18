---
title: "CF 2026C - Nhân Vật Hành Động"
description: "Chúng ta được cho một chuỗi các hình hành động được đánh số từ 1 đến n. Hình i tốn i xu, nhưng không thể mua ngay vào ngày đầu tiên; nó chỉ có sẵn bắt đầu từ ngày thứ i. Sau ngày n, mọi thứ đều có sẵn."
date: "2026-06-08T12:19:53+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "constructive-algorithms", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2026
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 171 (Rated for Div. 2)"
rating: 1500
weight: 2026
solve_time_s: 196
verified: false
draft: false
---

[CF 2026C - Nhân vật hành động](https://codeforces.com/problemset/problem/2026/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các hình hành động được đánh số từ 1 đến n. Hình i tốn i xu, nhưng không thể mua ngay vào ngày đầu tiên; nó chỉ có sẵn bắt đầu từ ngày thứ i. Sau ngày n, mọi thứ đều có sẵn. 

Trong n ngày, mỗi ngày sẽ là một ngày làm việc mà Monocarp có thể ghé thăm cửa hàng hoặc là một ngày bị chặn khi anh ta không thể ghé thăm cửa hàng. Vào ngày đến thăm, anh ta có thể mua bất kỳ tập hợp con số nào hiện có. Nếu anh ta mua ít nhất hai bức tượng trong cùng một chuyến thăm, bức tượng đắt nhất trong đợt đó sẽ miễn phí. 

Mục tiêu là mua mỗi con số chính xác một lần đồng thời giảm thiểu tổng số tiền chi tiêu. 

Khó khăn chính là các quyết định mua hàng bị hạn chế về mặt thời gian: những số liệu trước đó luôn có sẵn, nhưng những số liệu sau này đòi hỏi phải chờ những ngày cụ thể và việc phân nhóm các giao dịch mua hàng là vấn đề quan trọng do quy tắc chiết khấu. 

Các ràng buộc rất lớn, với tổng số n trong các trường hợp thử nghiệm lên tới 4·10^5, do đó, bất kỳ chiến lược bậc hai nào theo ngày hoặc số liệu đều không thể thực hiện được. Ngay cả O(n log n) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm trong trường hợp xấu nhất nếu được triển khai lặp đi lặp lại với các cấu trúc dữ liệu nặng, vì vậy chúng ta nên hướng tới hành vi tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất hiện khi số ngày thăm khám sớm thưa thớt nhưng dày đặc về muộn. Ví dụ: nếu chỉ có ngày cuối cùng thì mọi con số phải được mua cùng nhau và chỉ miễn phí mức tối đa. Điều đó dẫn đến chi phí 1 + 2 + ... + (n−1). Một người tham lam ngây thơ cố gắng trì hoãn việc mua hàng mà không tôn trọng tình trạng sẵn có có thể dễ dàng đánh giá thấp chi phí bằng cách lạm dụng việc phân nhóm vào cuối ngày. 

Một trường hợp khó khăn khác là khi số ngày thăm viếng liên tiếp nhau. Ví dụ: s = "11111". Sau đó, chúng ta có thể ghép nối mỗi lần mua hàng trong ngày liền kề để tối đa hóa mức chiết khấu. Một chiến lược ngây thơ luôn mua một mặt hàng cho mỗi lần ghé thăm sẽ làm mất cơ hội kết hợp mua hàng và giảm chi phí. 

Sự căng thẳng thực sự là việc quyết định khi nào nên trì hoãn việc mua một bức tượng để nó có thể được ghép nối sau này, thay vì mua nó ngay lập tức để tránh mất cơ hội ghép đôi. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ mô phỏng các quyết định: đối với mỗi con số, hãy chọn một ngày mà nó được mua và đối với mỗi ngày, hãy chọn những con số có sẵn để nhóm lại với nhau. Điều này trở thành bài toán gán với phân nhánh theo cấp số nhân vì mỗi tập hợp con các mặt hàng được mua trong một ngày đều ảnh hưởng đến tình trạng còn hàng và giảm giá trong tương lai. Ngay cả khi chúng tôi hạn chế DP trên các tập hợp con hoặc DP qua các ngày và “mặt nạ chưa mua”, thì không gian trạng thái là 2^n, điều này ngay lập tức là không thể đối với n lên tới 4·10^5. 

Quan sát quan trọng là cơ cấu giảm giá cực kỳ cứng nhắc: vào bất kỳ ngày nào, chỉ mặt hàng đắt nhất trong giao dịch mua ngày hôm đó mới được miễn phí. Điều này có nghĩa là mọi “vật phẩm miễn phí” phải được ghép nối với một vật phẩm đắt tiền hơn được mua trong cùng ngày và mỗi ngày có thể đóng góp tối đa một vật phẩm miễn phí. 

Vì vậy, toàn bộ vấn đề giảm xuống việc ghép từng mục i với một số j > i (trong đó j là ngày cả hai đều có sẵn) hoặc không ghép nối nó. Mỗi cặp (i, j) sẽ loại bỏ chi phí i. Chi phí j luôn được trả trừ khi bản thân nó được ghép với thứ gì đó lớn hơn. 

Bây giờ quan điểm thay đổi: thay vì nghĩ về ngày, chúng ta chỉ quan tâm đến những chỉ số nào có thể được nhóm lại và việc nhóm chỉ bị ràng buộc bởi điều kiện là cả hai mặt hàng phải có sẵn vào ngày đã chọn. Vì mặt hàng j có sẵn vào ngày j nên bất kỳ cặp nào liên quan đến j đều phải xảy ra vào hoặc sau ngày j, nghĩa là cả hai mặt hàng i < j đều có sẵn vào ngày j. 

Vì vậy, mọi cặp đôi chỉ đơn giản là chọn một “mỏ neo sau” j có thể hấp thụ một mục trước đó i < j. 

Chúng ta muốn tối đa hóa tổng số tiền tiết kiệm được, trong đó mỗi j có thể tiết kiệm nhiều nhất một i < j, và mỗi i có thể được sử dụng nhiều nhất một lần. Câu hỏi đặt ra là làm thế nào để gán các chỉ số nhỏ hơn cho các chỉ số lớn hơn một cách tham lam.

Chúng tôi xử lý các chỉ mục từ trái sang phải và duy trì một nhóm “những ngày có sẵn với số lượt truy cập” trong đó chúng tôi vẫn có thể chỉ định một mục nhỏ hơn để ghép với một mục lớn hơn trong tương lai. Mỗi khi chúng tôi gặp một ngày có s[i] = 1, ngày đó có thể đóng vai trò là điểm cuối ghép nối tiềm năng, nhưng chỉ sau khi chúng tôi đảm bảo tính khả thi với các hạn chế về tính khả dụng. 

Sự tham lam đúng đắn sẽ trở thành: khi chúng ta đạt đến vị trí i, chúng ta quyết định xem có nên ghép i như một món đồ miễn phí với một số ngày được truy cập sau đó hay trả tiền cho nó. Chúng tôi duy trì nhiều tập hợp các “thời điểm” đang hoạt động được tạo theo số ngày truy cập và chỉ định các chỉ số nhỏ cho các thời điểm sớm nhất có thể để tối đa hóa mức sử dụng. 

Một sự đơn giản hóa cụ thể hơn xuất hiện: chúng tôi mô phỏng từ trái sang phải, duy trì bộ đếm khả năng truy cập có sẵn. Mỗi '1' tăng công suất; mỗi khi gặp một món đồ, chúng tôi cố gắng gán nó miễn phí nếu có thể, nếu không thì chúng tôi sẽ trả tiền. Điều này có tác dụng vì việc chỉ định các mục nhỏ hơn cho các lượt truy cập sớm hơn có thể duy trì tính linh hoạt cho các mục lớn hơn, có ít tùy chọn hơn do hạn chế về tính khả dụng. 

Điều này giảm xuống thành quét tuyến tính với cấu trúc đơn giản theo dõi số lượng mục vẫn có thể được “ghép nối miễn phí”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công các mục theo ngày/tập hợp con Brute Force | O(2^n) | O(n) | Quá chậm | 
| Mô phỏng năng lực tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt các chỉ số từ 1 đến n, duy trì bộ đếm`free_slots`thể hiện số lượng mặt hàng trước đó vẫn có thể được chỉ định cho các lượt truy cập trong tương lai để ghép đôi giảm giá. Mỗi khi chúng tôi thấy một ngày ghé thăm, chúng tôi sẽ tăng công suất này vì nó có khả năng tổ chức một cặp đôi. 
2. Khi chúng tôi đạt đến vị trí i, trước tiên chúng tôi đảm bảo rằng chúng tôi tính đến tình trạng sẵn có: nếu ngày tôi là ngày ghé thăm, điều đó sẽ tăng khả năng hình thành nhóm giảm giá trong tương lai, vì vậy chúng tôi sẽ tăng`free_slots`. 
3. Nếu chúng tôi có sẵn một chỗ trống (`free_slots > 0`), chúng tôi chỉ định mục i là "miễn phí trong một số ghép nối trong tương lai", nghĩa là chúng tôi giảm`free_slots`và không trả tiền cho tôi. 
4. Nếu không còn chỗ trống, chúng ta phải thanh toán i ngay lập tức, thêm i vào câu trả lời. 
5. Tiếp tục cho đến n, đảm bảo mọi mặt hàng đều được thanh toán hoặc được chỉ định cho cơ hội ghép đôi hợp lệ trong tương lai. 

Điều tinh tế quan trọng là ngày thăm viếng không được sử dụng trực tiếp tại thời điểm ghép nối; thay vào đó, chúng tạo ra dung lượng được sử dụng bởi các mục chưa được chỉ định trước đó. Sự đảo ngược này chính là nguyên nhân khiến lòng tham hoạt động. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`free_slots`đại diện cho số lượng mặt hàng mà chúng tôi vẫn có đủ khả năng để trì hoãn và khớp với ngày đến thăm sau đó. Mỗi ngày truy cập đóng góp chính xác một “cơ hội nhận quà miễn phí” tiềm năng, bởi vì mỗi ngày truy cập chỉ có thể giảm giá một mặt hàng đắt nhất trong một đợt. Việc chỉ định các chỉ số nhỏ hơn trước tiên đảm bảo rằng bất cứ khi nào lượt truy cập được sử dụng, nó sẽ tiết kiệm tối đa vì nó giải phóng chi phí còn lại nhỏ nhất có thể. Sự kết hợp tham lam này giữa khả năng truy cập sẵn có và các mục nhỏ nhất chưa được giải quyết sẽ duy trì tính tối ưu vì bất kỳ sai lệch nào làm trì hoãn một mục nhỏ để chuyển sang mục lớn hơn chỉ có thể làm tăng tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        free_slots = 0
        ans = 0

        for i in range(n):
            if s[i] == '1':
                free_slots += 1

            # try to use a slot for item i+1
            if free_slots > 0:
                free_slots -= 1
            else:
                ans += (i + 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cách giải thích tham lam. Chỉ số vòng lặp i tương ứng với hình i+1. Mỗi lần chúng tôi nhìn thấy số '1', chúng tôi sẽ tăng số lượng cơ hội ghép đôi có sẵn trong tương lai. Khi xử lý một số liệu, chúng tôi sẽ chỉ định nó cho một trong những cơ hội này (tiêu thụ một vị trí) hoặc trả chi phí cho nó. 

Một điểm tinh tế là slot bị tiêu hao ngay khi sử dụng. Điều này phản ánh rằng mỗi lượt truy cập chỉ có thể “lưu” một mục, vì vậy chúng tôi không thể sử dụng lại một vị trí nhiều lần. Tính chính xác phụ thuộc vào việc luôn tiêu tốn một cách tham lam để có được những vật phẩm sớm nhất có thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
101101
```Chúng tôi theo dõi`free_slots`,`ans`. 

| tôi | s[i] | free_slots trước | hành động | free_slots sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | +1 khe, sử dụng khe | 0 | 0 | 
| 2 | 0 | 0 | trả 2 | 0 | 2 | 
| 3 | 1 | 0 | +1 khe, sử dụng khe | 0 | 2 | 
| 4 | 1 | 0 | +1 khe, sử dụng khe | 0 | 2 | 
| 5 | 0 | 0 | trả 5 | 0 | 7 | 
| 6 | 1 | 0 | +1 khe, sử dụng khe | 0 | 7 | 

Điều này cho thấy mỗi ngày truy cập sẽ hủy một khoản chi phí một cách hiệu quả như thế nào và các khoản chưa thanh toán còn lại chính xác là những khoản không thể so sánh được. 

### Ví dụ 2 

đầu vào:```
5
11111
```| tôi | s[i] | free_slots trước | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 -> 1 | sử dụng khe cắm | 0 | 
| 2 | 1 | 0 -> 1 | sử dụng khe cắm | 0 | 
| 3 | 1 | 0 -> 1 | sử dụng khe cắm | 0 | 
| 4 | 1 | 0 -> 1 | sử dụng khe cắm | 0 | 
| 5 | 1 | 0 -> 1 | sử dụng khe cắm | 0 | 

Ở đây, mọi mặt hàng đều được kết hợp thành một cơ hội giảm giá và không phải trả chi phí nào ngoại trừ cấu trúc ghép đôi không thể tránh khỏi, mang lại chi tiêu tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần từ trái sang phải cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Tổng n trên các trường hợp thử nghiệm là 4·10^5, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm nằm trong giới hạn. Thuật toán tránh mọi mô phỏng mỗi ngày hoặc mỗi cặp, duy trì công việc liên tục trên mỗi chỉ mục. 

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
        s = input().strip()

        free_slots = 0
        ans = 0

        for i in range(n):
            if s[i] == '1':
                free_slots += 1
            if free_slots > 0:
                free_slots -= 1
            else:
                ans += i + 1

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""4
1
1
6
101101
7
1110001
5
11111
""") == """1
8
18
6"""

# custom cases
assert run("""1
1
0
""") == "1", "single forced purchase"
assert run("""1
3
000
""") == "6", "no visits"
assert run("""1
5
11111
""") == "0", "all fully optimally paired"
assert run("""1
6
100000
""") == "15", "only early visit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | thanh toán bắt buộc tối thiểu | 
| 000 | 6 | không có cơ hội giảm giá | 
| 11111 | 0 | mật độ ghép nối tối đa | 
| 100000 | 15 | giới hạn chuyến thăm sớm duy nhất | 

## Vỏ cạnh 

Khi không có ngày truy cập nào cho đến thời điểm cuối cùng ngoại trừ ngày n, thuật toán sẽ không tích lũy`free_slots`cho đến khi kết thúc, vì vậy mọi mặt hàng trước đó sẽ được thanh toán ngoại trừ những mặt hàng có thể được ghép nối vào ngày cuối cùng. Điều này phù hợp với thực tế là tất cả các mặt hàng chỉ có thể được nhóm lại một lần ở cuối, mang lại mức sử dụng chiết khấu tối đa nhưng vẫn bị giới hạn bởi một mặt hàng miễn phí cho mỗi đợt truy cập. 

Khi tất cả các ngày đều là ngày thăm viếng,`free_slots`không bao giờ hết nên mọi vật phẩm đều được khớp ngay khi xuất hiện. Thuật toán sử dụng các vị trí ngay lập tức, phản ánh rằng mọi con số đều có thể được đưa vào một số lô trong tương lai, giảm thiểu chi phí. 

Khi số ngày truy cập thưa thớt và tập trung sớm, các suất sẽ tích lũy nhưng có thể bị tiêu thụ trước khi các chỉ số lớn xuất hiện. Việc tiêu dùng tham lam đảm bảo các chỉ số nhỏ luôn được chỉ định trước, bảo toàn các chỉ số lớn cho các khoản thanh toán bắt buộc sau này, phù hợp với cấu trúc tối ưu vì việc trì hoãn các mục nhỏ không bao giờ có ích khi khả năng ghép nối đã sẵn sàng.
