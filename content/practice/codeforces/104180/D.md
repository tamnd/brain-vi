---
title: "CF 104180D - Phòng tập Grumble"
description: "Chúng ta được cung cấp một chuỗi nước tăng lực mà Alberto tiêu thụ theo một thứ tự cố định. Mỗi đồ uống đóng góp một lượng năng lượng nhất định và khi bắt đầu uống, anh ta sẽ uống hết nó trước khi tiếp tục."
date: "2026-07-02T00:43:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 78
verified: false
draft: false
---

[CF 104180D - Phòng tập thể dục Grumble](https://codeforces.com/problemset/problem/104180/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nước tăng lực mà Alberto tiêu thụ theo một thứ tự cố định. Mỗi đồ uống đóng góp một lượng năng lượng nhất định và khi bắt đầu uống, anh ta sẽ uống hết nó trước khi tiếp tục. Năng lượng này được tích lũy và dành cho việc chống đẩy bên trong cấu trúc tập luyện cũng được cố định. 

Một “bộ” tập luyện duy nhất bao gồm thực hiện các động tác chống đẩy với mức tiêu hao năng lượng ngày càng tăng. Lần chống đẩy đầu tiên tiêu tốn 1 đơn vị năng lượng, lần thứ hai tốn 2 đơn vị năng lượng, v.v. cho đến M lần đẩy, trong đó lần đẩy thứ k tiêu tốn k năng lượng. Alberto phải hoàn thành một hiệp đầy đủ để đếm nó và nếu tại bất kỳ thời điểm nào anh ta không đủ khả năng thực hiện động tác chống đẩy tiếp theo trong hiệp hiện tại, hiệp đó được coi là thất bại và anh ta dừng lại ngay lập tức. Sau khi hoàn thành một bộ đầy đủ, năng lượng của anh ấy sẽ đặt lại về 0. 

Nhiệm vụ là xác định xem anh ta có thể hoàn thành bao nhiêu hiệp đầy đủ trước khi cạn kiệt năng lượng từ đồ uống tiêu thụ theo thứ tự. 

Quan sát chính từ các ràng buộc là N có thể lên tới 100000 và M lên tới 1000. Điều này có nghĩa là giải pháp mô phỏng trực tiếp mọi lần đẩy lên trên mỗi đơn vị năng lượng sẽ quá chậm trong trường hợp xấu nhất, vì điều đó sẽ giảm xuống khoảng O(NM) hoặc tệ hơn. Tuy nhiên, N lớn trong khi M tương đối nhỏ, điều này gợi ý rằng việc ghi sổ theo mỗi lần uống hoặc theo bộ với công việc được khấu hao là O(1) hoặc O(M) là có thể chấp nhận được. 

Một sai lầm ngây thơ sẽ là mô phỏng từng lần chống đẩy trong khi tiêu thụ nước tăng lực tăng dần. Một cạm bẫy tinh vi khác là việc đặt lại năng lượng không chính xác giữa các điểm tiêu thụ một phần, đặc biệt là khi đồ uống kết thúc chính xác ở ranh giới của một bộ. 

Một trường hợp thất bại cụ thể cho mô phỏng đẩy lên đơn giản: 

đầu vào:```
1 3
10
```Một mô phỏng bất cẩn có thể làm giảm năng lượng cho mỗi lần đẩy liên tục và không nhận ra rằng toàn bộ tam giác có giá 1 + 2 + 3 = 6 hoàn toàn phù hợp, khi đó 4 năng lượng còn lại không thể bắt đầu một hiệp mới (cần lại 1+2+3 nhưng phải đặt lại cho mỗi hiệp). Kết quả đầu ra đúng là 1. Bất kỳ hoạt động triển khai nào không được đặt lại chính xác sau khi tập đầy đủ hoặc xử lý sai một phần tiến trình của các đồ uống đều có thể dễ dàng bị tính sai. 

Một trường hợp cạnh khác: 

đầu vào:```
2 3
3 3
```Ở đây mỗi đồ uống tuy nhỏ nhưng gộp lại cho phép đúng một bộ đầy đủ. Một cách tiếp cận tham lam “đặt lại quá sớm” cho mỗi lần uống thay vì mỗi bộ sẽ tạo ra 0 hoặc 2 không chính xác tùy thuộc vào chi tiết triển khai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng toàn bộ quá trình của Alberto chính xác như được mô tả. Chúng tôi duy trì năng lượng hiện tại, lặp đi lặp lại qua đồ uống và đối với mỗi đơn vị năng lượng, chúng tôi mô phỏng các lần chống đẩy một cách tuần tự. Đối với mỗi bộ, chúng tôi theo dõi chi phí yêu cầu tiếp theo k, giảm năng lượng tương ứng và tăng k cho đến khi chúng tôi hoàn thành bộ hoặc thất bại. Nếu chúng tôi thất bại giữa hiệp, chúng tôi sẽ dừng hoàn toàn. Điều này đúng vì nó phản ánh chính xác định nghĩa vấn đề. 

Tuy nhiên, mô phỏng này có thể trở nên tốn kém vì mỗi đơn vị năng lượng có thể tương ứng với nhiều lần đẩy và mỗi hiệp yêu cầu tổng cộng M lần đẩy. Trong trường hợp xấu nhất, N lớn và M là 1000, dẫn đến hành vi gần như O(NM) hoặc tệ hơn, đặc biệt nếu giá trị năng lượng lớn và được xử lý nhiều lần trong mỗi lần đẩy. 

Điểm mấu chốt là mỗi bộ có tổng yêu cầu năng lượng cố định: 1 + 2 + … + M = M(M+1)/2. Điều này làm giảm mỗi bộ thành một kiểm tra ngưỡng duy nhất thay vì mô phỏng tăng dần. Thay vì mô phỏng động tác chống đẩy, chúng ta tích lũy năng lượng từ đồ uống cho đến khi đạt hoặc vượt ngưỡng này. Mỗi lần đạt được nó, chúng tôi đếm một bộ đầy đủ và trừ đi chính xác số lượng cần thiết, đặt lại năng lượng về 0. 

Điều này biến vấn đề thành năng lượng tích lũy liên tục cho đến khi đạt được mục tiêu cố định, sau đó đặt lại, đây là mô hình tích lũy tham lam tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N·M) | O(1) | Quá chậm | 
| Tích lũy ngưỡng tham lam | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đặt S = M(M+1)/2, tổng năng lượng cần thiết cho một bộ đầy đủ. 

1. Tính S một lần khi bắt đầu. Điều này thể hiện toàn bộ chi phí để hoàn thành một hiệp đấu mà không cần mô phỏng các lần đẩy riêng lẻ. 
2. Khởi tạo current_energy = 0 và Complete_sets = 0. 
3. Lặp lại từng giá trị nước tăng lực E[i]. 
4. Thêm E[i] vào current_energy, vì Alberto uống đầy đủ đồ uống theo thứ tự. 
5. Trong khi current_energy ít nhất là S, hãy trừ S khỏi current_energy và tăng số lần hoàn thành. 

Điều này thể hiện việc hoàn thành một hoặc nhiều hiệp đầy đủ bằng cách sử dụng năng lượng tích lũy. 
6. Tiếp tục cho đến khi tất cả đồ uống được xử lý. 
7. Xuất ra các tập hợp đã hoàn thành. 

Bước suy luận quan trọng là coi mỗi bộ như một “điểm kiểm tra năng lượng” cố định. Sau khi tích lũy đủ năng lượng, nhiều hiệp có thể được hoàn thành trong một bước nếu một ly lớn duy nhất đẩy tổng lượng vượt xa S. 

### Tại sao nó hoạt động 

Mỗi bộ là độc lập vì năng lượng sẽ đặt lại về 0 sau khi hoàn thành. Điều này có nghĩa là tiến trình từng phần trong một tập hợp không được chuyển tiếp. Do đó, hệ thống hoạt động giống như liên tục đổ đầy một xô có dung tích S bằng cách sử dụng các khối nước (đồ uống) đổ vào. Mỗi khi thùng đầy, chúng tôi đếm một lần hoàn thành và làm trống nó. Vì mức sử dụng năng lượng bên trong một bộ ngày càng tăng nhưng mang tính xác định nên tổng chi phí của nó là cố định và không phụ thuộc vào cách năng lượng đến. Điều này làm cho sự tích lũy tham lam trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M = map(int, input().split())
    E = list(map(int, input().split()))

    target = M * (M + 1) // 2
    energy = 0
    sets = 0

    for x in E:
        energy += x
        while energy >= target:
            energy -= target
            sets += 1

    print(sets)

if __name__ == "__main__":
    main()
```Giải pháp chỉ dựa vào việc duy trì hai biến, năng lượng tích lũy hiện tại và số lượng bộ hoàn chỉnh. Số tam giác được tính toán trước một lần, tránh tính tổng lặp lại. Vòng lặp while bên trong có thể trông có vẻ đáng lo ngại, nhưng mỗi phép trừ mục tiêu tương ứng với một tập hợp đã hoàn thành, do đó, trong toàn bộ quá trình thực thi, nó chạy tối đa nhiều lần bằng số lượng các tập hợp đã hoàn thành, khiến nó được phân bổ tuyến tính. 

Một lỗi triển khai phổ biến là sử dụng một if thay vì while khi trừ mục tiêu. Điều đó sẽ thất bại khi một ly lớn hoàn thành nhiều hiệp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
2 20 80 4
```| Uống | Thêm năng lượng | Năng lượng hiện tại | Bộ đã hoàn thành | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | +2 | 2 | 0 | không có bộ | 
| 2 | +20 | 22 | 1 | 22 ≥ 15, đủ 1 bộ | 
| 3 | +80 | 87 | 6 | 87 ≥ 15 lần | 
| 4 | +4 | 1 | 6 | phần còn lại | 

Đầu ra cuối cùng là 6. 

Dấu vết này cho thấy mức năng lượng dư thừa lớn tạo ra nhiều lần hoàn thành trong một bước duy nhất, việc củng cố phép trừ lặp đi lặp lại đó là cần thiết. 

### Mẫu 2 

đầu vào:```
3 3
20 5 2
```| Uống | Thêm năng lượng | Năng lượng hiện tại | Bộ đã hoàn thành | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | +20 | 20 | 1 | hoàn thành 1 bộ (S=6) | 
| 2 | +5 | 19 | 4 | ba bộ nữa | 
| 3 | +2 | 5 | 4 | không thể hoàn thành | 

Đầu ra cuối cùng là 4. 

Điều này chứng tỏ rằng năng lượng được truyền qua đồ uống là cần thiết và việc đặt lại chỉ xảy ra khi tiêu thụ hết ngưỡng đã đặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) khấu hao | Mỗi đồ uống được chế biến một lần và mỗi bộ hoàn thành sẽ giảm năng lượng một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì N lên tới 100000 và tất cả các phép toán đều là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return main() or ""

# provided samples
assert run("4 5\n2 20 80 4\n") == "6\n", "sample 1"
assert run("3 3\n20 5 2\n") == "4\n", "sample 2"

# minimum case
assert run("1 1\n0\n") == "0\n", "single zero"

# exact one set
assert run("1 3\n6\n") == "1\n", "exact triangular"

# multiple sets in one drink
assert run("1 3\n100\n") == "16\n", "multiple completions"

# alternating small values
assert run("5 2\n1 1 1 1 1\n") == "2\n", "small accumulation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/0 | 0 | trường hợp không có tiến triển | 
| 1 3/6 | 1 | ngưỡng chính xác | 
| 1 3/100 | 16 | nhảy nhiều hiệp | 
| 5 2 / tất cả 1s | 2 | tích lũy trên nhiều đầu vào nhỏ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một đồ uống chứa đủ năng lượng để hoàn thành nhiều hiệp. Ví dụ: 

đầu vào:```
1 4
100
```Ở đây M = 4 nên S = 10. Bắt đầu từ 100 năng lượng, thuật toán liên tục trừ 10, tạo ra 10 bộ đầy đủ và để lại 0 phần dư. Vòng lặp while xử lý việc này một cách chính xác, trong khi việc triển khai dựa trên if sẽ chỉ tính một bộ và để lại năng lượng dư thừa không chính xác. 

Một trường hợp khác là khi năng lượng khớp chính xác với ngưỡng: 

đầu vào:```
2 3
3 3
```S = 6. Sau lần uống đầu tiên, năng lượng là 3. Sau lần uống thứ hai, năng lượng trở thành 6, kích hoạt chính xác một bộ và đặt lại về 0. Bất kỳ lỗi sai sót nào khi so sánh (sử dụng > thay vì >=) sẽ không đếm được bộ này một cách sai lầm.
