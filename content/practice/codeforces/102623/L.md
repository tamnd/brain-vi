---
title: "CF 102623L - Vé số"
description: "Chúng tôi có một bộ sưu tập các thẻ chữ số. Đối với mỗi chữ số từ 0 đến 9, dữ liệu đầu vào cho chúng ta biết có bao nhiêu bản sao của chữ số đó. Chúng tôi có thể chọn bất kỳ tập hợp con nào của các thẻ này và sắp xếp các chữ số đã chọn thành số thập phân."
date: "2026-08-02T14:17:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "L"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 65
verified: true
draft: false
---

[CF 102623L - Vé xổ số](https://codeforces.com/problemset/problem/102623/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các thẻ chữ số. Đối với mỗi chữ số từ 0 đến 9, dữ liệu đầu vào cho chúng ta biết có bao nhiêu bản sao của chữ số đó. Chúng tôi có thể chọn bất kỳ tập hợp con nào của các thẻ này và sắp xếp các chữ số đã chọn thành số thập phân. Trong số tất cả các số có thể lập được và chia hết cho 4, chúng ta cần số lớn nhất. Nếu không tồn tại số dương hợp lệ, chúng ta vẫn phải xem xét liệu số 0 có thể được tạo thành hay không, vì số 0 chia hết cho 4 và định dạng đầu ra cho phép điều đó. 

Khó khăn không phải là tạo ra một số chia hết. Khó khăn là tối đa hóa giá trị của nó trong khi quyết định nên giữ lá bài nào và bỏ lá bài nào. Số có nhiều chữ số luôn lớn hơn số có ít chữ số hơn nên ưu tiên hàng đầu là sử dụng càng nhiều thẻ càng tốt. Sau đó, mục tiêu còn lại là sắp xếp các chữ số đó theo thứ tự lớn nhất có thể. 

Số lượng trường hợp thử nghiệm có thể lên tới 300000, nhưng tổng số thẻ trong tất cả các thử nghiệm nhiều nhất là 300000. Điều này có nghĩa là một thuật toán xử lý mỗi thẻ với số lần không đổi là phù hợp. Một giải pháp thử tất cả các tập hợp con hoặc tất cả các cách sắp xếp có thể là không thể vì số lượng khả năng tăng theo cấp số nhân hoặc theo giai thừa. Ngay cả việc xây dựng lại nhiều số ứng cử viên cũng trở nên quá tốn kém khi một trường hợp thử nghiệm chứa 100000 thẻ. 

Các trường hợp chính xuất phát từ sự tương tác giữa khả năng chia hết và số 0 đứng đầu. Giải pháp chỉ sắp xếp các chữ số giảm dần và kiểm tra kết quả có thể không thành công vì cách sắp xếp lớn nhất có thể không chia hết cho 4. Giải pháp luôn loại bỏ một hoặc hai chữ số mà không xem xét đến tác động giá trị cũng có thể thất bại. 

Ví dụ: nếu đầu vào là:```
0 1 1 0 0 0 0 0 0 0
```các thẻ có chữ số 1 và 2. Sắp xếp cho 21, nhưng 21 không chia hết cho 4. Câu trả lời đúng là:```
12
```vì hai chữ số cuối quyết định số chia hết. 

Một trường hợp khác là:```
1 0 0 2 0 0 0 0 0 0
```Các chữ số có sẵn là 0, 3 và 3. Không có bội số có hai chữ số của 4, nhưng số có một chữ số 0 là hợp lệ. Câu trả lời là:```
0
```Việc triển khai bất cẩn chỉ tìm kiếm các kết thúc có hai chữ số sẽ in sai`-1`. 

Trường hợp khó khăn cuối cùng là khi câu trả lời chứa nhiều số không. Ví dụ:```
2 0 0 0 0 0 0 0 0 0
```Đầu ra đúng là:```
0
```vì số 0 đứng đầu không tạo ra số lớn hơn. In tất cả các số không sẽ vi phạm cách biểu diễn được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra mọi tập hợp con thẻ có thể có, sắp xếp các chữ số đã chọn theo thứ tự giảm dần và kiểm tra xem số kết quả có chia hết cho 4 hay không. Điều này đúng vì mọi ứng cử viên có thể đều được kiểm tra và việc sắp xếp sẽ đưa ra cách sắp xếp lớn nhất cho tập hợp con đã chọn đó. Tuy nhiên, với 100000 thẻ thì việc xem xét các tập hợp con là không thể. Số lượng các tập hợp con là theo cấp số nhân, và thậm chí việc hạn chế sự chú ý đến các kết thúc có thể xảy ra vẫn sẽ khiến có quá nhiều ứng cử viên. 

Quan sát quan trọng xuất phát từ quy tắc chia hết cho 4. Một số thập phân chia hết cho 4 khi và chỉ khi hai chữ số cuối của nó tạo thành một số chia hết cho 4. Tất cả các chữ số trước hai vị trí cuối cùng đó chỉ ảnh hưởng đến kích thước của số đó chứ không ảnh hưởng đến khả năng chia hết của nó. 

Điều này thay đổi vấn đề hoàn toàn. Chúng ta không cần xây dựng nhiều ứng viên. Chúng ta chỉ cần quyết định một hoặc hai chữ số nào sẽ được đặt ở cuối. Sau khi cố định phần cuối, nên sử dụng mọi thẻ còn lại vì việc thêm một chữ số khác luôn tạo ra số lớn hơn. Các chữ số còn lại nên được đặt theo thứ tự giảm dần. 

Chỉ có 100 kết thúc có hai chữ số từ 00 đến 99 và chỉ có 10 kết thúc có thể có một chữ số. Chúng tôi có thể kiểm tra tất cả chúng. Đối với mọi kết thúc có thể xảy ra, chúng tôi kiểm tra xem số lượng chữ số được yêu cầu có tồn tại hay không. Sau đó, chúng tôi xóa các chữ số đó, đặt tất cả các chữ số còn lại theo thứ tự giảm dần và so sánh số kết quả với câu trả lời đúng nhất hiện tại. 

Việc so sánh giữa các ứng viên cũng đơn giản. Số có nhiều chữ số hơn thì lớn hơn. Nếu hai ứng cử viên có cùng độ dài, việc so sánh từ điển các chuỗi của họ sẽ cho số lượng lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ theo số lượng thẻ | Hàm mũ | Quá chậm | 
| Tối ưu | O(100 + 10) cho mỗi trường hợp thử nghiệm | O(1) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ số đếm của từng chữ số. Chúng ta sẽ sử dụng số đếm này để kiểm tra xem liệu có thể tạo ra một kết thúc khả dĩ mà không cần thực sự xây dựng mọi sự sắp xếp hay không. 
2. Hãy thử mọi hậu tố có hai chữ số có thể từ 00 đến 99. Nếu một hậu tố chia hết cho 4 và có sẵn các chữ số của nó, hãy tạm thời xóa các chữ số đó. Tốt nhất nên đặt các chữ số còn lại theo thứ tự giảm dần vì chúng tạo thành tiền tố, trong đó mọi chữ số lớn hơn trước đó sẽ làm tăng số cuối cùng. 
3. Ngoài ra hãy thử mọi số có một chữ số có thể. Điều này xử lý các số chỉ có một thẻ, đặc biệt là trường hợp đặc biệt khi kết quả hợp lệ duy nhất là 0. 
4. Với mỗi lựa chọn hợp lệ, hãy xây dựng chuỗi ứng cử viên. Loại bỏ các số 0 ở đầu không cần thiết bằng cách chuyển đổi kết quả hoàn toàn bằng 0 thành`"0"`. 
5. Giữ ứng cử viên lớn nhất bằng cách trước tiên so sánh độ dài và sau đó so sánh các chuỗi khi độ dài khớp nhau. 

Lý do chỉ kiểm tra hậu tố có tác dụng là vì mọi số thập phân hợp lệ chia hết cho 4 đều có hậu tố cuối cùng có một hoặc hai chữ số hợp lệ. Khi các chữ số cuối cùng đó được chọn, không còn ràng buộc nào đối với tiền tố, do đó, sử dụng tất cả các thẻ còn lại theo thứ tự giảm dần luôn là tối ưu. 

Tại sao nó hoạt động: 

Hãy xem xét câu trả lời tối ưu. Hai chữ số cuối cùng của nó, hoặc một chữ số duy nhất nếu nó có độ dài bằng 1, phải là một trong những phần cuối được thuật toán kiểm tra. Khi thuật toán đạt đến phần cuối đó, nó sẽ sử dụng chính xác các chữ số giống nhau cho hậu tố. Tất cả các thẻ có sẵn khác có thể được đưa vào tiền tố một cách an toàn vì việc thêm các chữ số trước hậu tố luôn làm tăng số lượng. Sắp xếp tiền tố đó theo thứ tự giảm dần sẽ tạo ra tiền tố lớn nhất có thể. Do đó, thuật toán tạo ra một ứng cử viên ít nhất bằng câu trả lời tối ưu và vì câu trả lời tối ưu nằm trong số các ứng cử viên được kiểm tra nên mức tối đa được chọn chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(cnt):
    best = None

    def update(s):
        nonlocal best
        if len(s) > 1 and s[0] == '0':
            s = s.lstrip('0')
            if not s:
                s = "0"
        if best is None or len(s) > len(best) or (len(s) == len(best) and s > best):
            best = s

    for x in range(100):
        if x % 4 != 0:
            continue

        a = x // 10
        b = x % 10

        need = [0] * 10
        need[a] += 1
        need[b] += 1

        ok = True
        for d in range(10):
            if need[d] > cnt[d]:
                ok = False
                break

        if ok:
            cur = cnt[:]
            cur[a] -= 1
            cur[b] -= 1

            s = []
            for d in range(9, -1, -1):
                s.append(str(d) * cur[d])

            s.append(str(a))
            s.append(str(b))
            update(''.join(s))

    for d in range(10):
        if cnt[d] > 0 and d % 4 == 0:
            cur = cnt[:]
            cur[d] -= 1

            s = []
            for x in range(9, -1, -1):
                s.append(str(x) * cur[x])

            s.append(str(d))
            update(''.join(s))

    return best if best is not None else "-1"

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        cnt = list(map(int, input().split()))
        ans.append(solve_case(cnt))

    print('\n'.join(ans))

if __name__ == "__main__":
    main()
```
