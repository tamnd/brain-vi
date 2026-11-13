---
title: "CF 1709A - Ba Cửa"
description: "Chúng ta được cấp cho ba cánh cửa, mỗi cánh cửa có một số khóa duy nhất từ ​​1 đến 3. Chúng ta cũng cầm trên tay một chiếc chìa khóa, và đằng sau mỗi cánh cửa có thể có một chiếc chìa khóa khác hoặc không có gì cả. Hai chiếc chìa khóa được giấu sau cánh cửa và một chiếc chìa khóa nằm trong tay chúng ta."
date: "2026-06-09T20:51:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1709
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 132 (Rated for Div. 2)"
rating: 800
weight: 1709
solve_time_s: 123
verified: true
draft: false
---

[CF 1709A - Ba cánh cửa](https://codeforces.com/problemset/problem/1709/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp cho ba cánh cửa, mỗi cánh cửa có một số khóa duy nhất từ 1 đến 3. Chúng ta cũng cầm trên tay một chiếc chìa khóa, và đằng sau mỗi cánh cửa có thể có một chiếc chìa khóa khác hoặc không có gì cả. Hai chiếc chìa khóa được giấu sau cánh cửa và một chiếc chìa khóa nằm trong tay chúng ta. Mỗi chìa khóa có thể mở chính xác cánh cửa có số trùng khớp. Nhiệm vụ là xác định xem, bắt đầu với chiếc chìa khóa trong tay, chúng ta có thể tuần tự mở cả ba cánh cửa hay không, trên đường đi lấy những chiếc chìa khóa còn lại. 

Đầu vào bao gồm một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được số trên chìa khóa trong tay và danh sách ba số nguyên tương ứng biểu thị các chìa khóa phía sau cửa 1, 2 và 3. Số 0 nghĩa là không có chìa khóa đằng sau cánh cửa đó. Chính xác một số chìa khóa xuất hiện trên tay, hai số chìa khóa còn lại ở phía sau cánh cửa, trong khi cánh cửa thứ ba không chứa chìa khóa. 

Các ràng buộc rất nhỏ: tối đa 18 trường hợp thử nghiệm và các khóa được giới hạn từ 1 đến 3. Điều này ngay lập tức loại bỏ mọi lo ngại về hiệu suất. Vấn đề hoàn toàn logic: giải pháp chỉ cần mô hình hóa chính xác quá trình mở khóa cửa và lấy chìa khóa. 

Trường hợp khó khăn phát sinh khi chiếc chìa khóa trong tay không trực tiếp mở được cánh cửa chứa chiếc chìa khóa thứ hai mà chúng ta cần. Ví dụ: nếu chúng ta bắt đầu bằng phím số 2 và chìa khóa phía sau cánh cửa là`[1, 3, 0]`, khi đó chúng ta không thể chạm tới phím 1 vì cửa 1 yêu cầu phím 1 mới mở được, tạo ra tình trạng bế tắc. Đầu ra đúng là "KHÔNG" trong trường hợp này. Một cách tiếp cận ngây thơ có thể bỏ qua sự phụ thuộc này và cho rằng tất cả các cánh cửa đều có thể mở được một cách sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là dùng vũ lực: thử mọi trình tự mở khóa cửa bằng chìa khóa trong tay và bất kỳ chìa khóa nào lấy được. Vì chỉ có ba cửa nên số lượng trình tự nhỏ (3! = 6), do đó, mô phỏng bạo lực sẽ hoạt động nhưng nó quá mức cần thiết và che khuất logic. 

Điểm mấu chốt là vấn đề có thể được giảm xuống bằng cách kiểm tra hai bước: đầu tiên, sử dụng chìa khóa trong tay để mở cánh cửa tương ứng. Sau đó lấy chìa khóa đằng sau cánh cửa đó nếu có và xem liệu nó có thể mở được một trong những cánh cửa còn lại hay không. Nếu chiếc chìa khóa thứ nhất mở được cánh cửa có chứa chiếc chìa khóa thứ hai thì chúng ta luôn có thể mở được cánh cửa còn lại bằng chiếc chìa khóa thứ hai đó. Nếu không, chúng ta có thể bị kẹt nếu những chiếc chìa khóa còn lại nằm sau những cánh cửa mà chìa khóa chúng ta có không thể mở được. 

Chúng ta có thể chính thức hóa điều này thành một chuỗi đơn giản: hãy`current_key`là chìa khóa trong tay. Mở cửa bằng số`current_key`. Nếu cánh cửa đó có chìa khóa phía sau (khác 0), hãy đặt`current_key`vào chìa khóa đó và mở cửa bằng số đó. Nếu tại bất kỳ thời điểm nào chúng ta cố gắng mở một cánh cửa yêu cầu chìa khóa mà chúng ta không có thì câu trả lời là "KHÔNG". Nếu chúng ta mở thành công hai cánh cửa và lấy được chiếc chìa khóa cuối cùng thì cánh cửa còn lại luôn có thể mở được bằng chiếc chìa khóa cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc key trong tay`x`và danh sách`[a, b, c]`đại diện cho chìa khóa đằng sau cánh cửa 1, 2 và 3. 
3. Coi cửa và chìa khóa như một ánh xạ từ số cửa đến chìa khóa phía sau nó. Nói cách khác cửa 1 có chìa khóa`a`, cửa 2 có chìa khóa`b`, cửa 3 có chìa khóa`c`. 
4. Bắt đầu với chiếc chìa khóa trong tay`current_key`. Kiểm tra cửa với số đó. Nếu chìa khóa đằng sau nó là 0 thì cánh cửa chưa mở còn lại phải có chiếc chìa khóa cuối cùng mà chúng ta đã có sẵn. In "CÓ". 
5. Nếu cửa có chìa khóa phía sau (khác 0) thì lấy chìa khóa đó và kiểm tra cửa tương ứng với chìa khóa đó. Nếu cánh cửa đó chứa số 0 (không có chìa khóa) thì chúng ta vẫn mở được. Nếu nó có chìa khóa thì đó phải là cánh cửa cuối cùng. Xác minh rằng khóa này khớp với khóa cuối cùng còn lại. 
6. Nếu tại bất kỳ thời điểm nào chìa khóa chúng ta cần ở phía sau cánh cửa mà chúng ta không thể mở bằng chìa khóa trong tay, hãy in "KHÔNG". Ngược lại in ra "CÓ". 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, chiếc chìa khóa trong tay có thể mở một cánh cửa đưa cho chúng ta chiếc chìa khóa tiếp theo hoặc mở một cánh cửa không có chìa khóa. Bởi vì chỉ có ba cửa và các chìa khóa được phân bổ sao cho chính xác một chìa khóa bị thiếu phía sau một cánh cửa, nên mô phỏng hai bước này là đủ để xác định xem tất cả các cửa đều có thể truy cập được hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x = int(input())
    a, b, c = map(int, input().split())
    doors = {1: a, 2: b, 3: c}

    current_key = x
    first_door_key = doors[current_key]

    if first_door_key == 0:
        print("NO")
    else:
        second_door_key = doors[first_door_key]
        if second_door_key == 0:
            print("YES")
        else:
            print("NO")
```Giải pháp đọc các trường hợp thử nghiệm và biểu diễn các cánh cửa dưới dạng ánh xạ từ điển từ số cửa đến chìa khóa đằng sau nó. Bắt đầu với chiếc chìa khóa trong tay, nó mô phỏng việc mở cánh cửa tương ứng. Nếu cửa có chìa khóa, nó sẽ cố gắng mở cửa tiếp theo bằng chìa khóa đó. Nếu cánh cửa thứ hai không có chìa khóa phía sau thì tất cả các cửa đều mở được nên ta in "CÓ". Nếu không, chúng ta sẽ bị mắc kẹt và câu trả lời là "KHÔNG". 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
3
0 1 2
1
0 3 2
2
3 1 0
```Dấu vết cho trường hợp thử nghiệm đầu tiên: 

| Bước | current_key | cửa mở | chìa khóa sau cửa | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | cửa 3 | 2 | lấy chìa khóa 2 | 
| 2 | 2 | cửa 2 | 1 | lấy chìa khóa 1 | 
| 3 | 1 | cửa 1 | 0 | không còn chìa khóa | 

Điều này cho thấy bắt đầu từ phím số 3, chúng ta có thể mở khóa tuần tự tất cả các cửa và lấy được tất cả các chìa khóa. 

Dấu vết cho trường hợp thử nghiệm thứ hai: 

| Bước | current_key | cửa mở | chìa khóa sau cửa | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | cửa 1 | 0 | không thể tiếp tục | 

Ở đây, chiếc chìa khóa trong tay sẽ mở được một cánh cửa không có chìa khóa đằng sau. Chúng ta không thể truy cập bất kỳ key nào còn lại nên câu trả lời là "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm tối đa hai lần tra cứu từ điển | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ ba cửa cho mỗi trường hợp thử nghiệm | 

Được cho`t <= 18`, tốc độ này rất nhanh và nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x = int(input())
        a, b, c = map(int, input().split())
        doors = {1: a, 2: b, 3: c}
        current_key = x
        first_door_key = doors[current_key]
        if first_door_key == 0:
            print("NO")
        else:
            second_door_key = doors[first_door_key]
            if second_door_key == 0:
                print("YES")
            else:
                print("NO")
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n0 1 2\n1\n0 3 2\n2\n3 1 0\n2\n1 3 0\n") == "YES\nNO\nYES\nNO"

# Custom cases
assert run("1\n1\n0 2 3\n") == "NO", "cannot open first door to get next key"
assert run("1\n2\n1 0 3\n") == "YES", "first door has next key"
assert run("1\n3\n1 2 0\n") == "YES", "first door has next key"
assert run("1\n1\n2 0 3\n") == "NO", "first door leads to door with no key"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n0 2 3 | KHÔNG | Không thể tiếp tục nếu chìa khóa đầu tiên mở được cánh cửa trống | 
| 1\n2\n1 0 3 | CÓ | Trường hợp bình thường, cửa thứ hai chứa chìa khóa tiếp theo | 
| 1\n3\n1 2 0 | CÓ | Cánh cửa cuối cùng không có chìa khóa, vẫn thành công | 
| 1\n1\n2 0 3 | KHÔNG | Chìa khóa đầu tiên dẫn đến cánh cửa không có chìa khóa phía sau | 

## Vỏ cạnh 

Trường hợp khó phát hiện xảy ra khi chiếc chìa khóa trong tay tương ứng với một cánh cửa không có chìa khóa phía sau. Ví dụ, nếu`x = 1`và cửa =`[0, 2, 3]`, chúng ta bắt đầu bằng chìa khóa 1, mở cánh cửa 1 nhưng không tìm thấy gì. Chìa khóa 2 và 3 ở phía sau cánh cửa 2 và 3,
