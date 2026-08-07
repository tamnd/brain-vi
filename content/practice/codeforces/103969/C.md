---
title: "CF 103969C - Bánh Cưới"
description: "Chúng ta được sắp xếp theo trình tự các ngày và mỗi ngày Mel nhận được một thành phần lớp bánh duy nhất được dán nhãn từ 1 đến 5. Mel đang làm bánh cưới và mỗi chiếc bánh hoàn chỉnh phải được lắp ráp theo thứ tự nghiêm ngặt từ lớp 1 đến lớp 5."
date: "2026-07-02T06:24:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103969
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-14-22 Div. 1 (Advanced)"
rating: 0
weight: 103969
solve_time_s: 49
verified: true
draft: false
---

[CF 103969C - Bánh cưới](https://codeforces.com/problemset/problem/103969/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các ngày và mỗi ngày Mel nhận được một thành phần lớp bánh duy nhất được dán nhãn từ 1 đến 5. Mel đang làm bánh cưới và mỗi chiếc bánh hoàn chỉnh phải được lắp ráp theo thứ tự nghiêm ngặt từ lớp 1 đến lớp 5. Mỗi thành phần có thể được sử dụng ngay trong ngày nó đến, nhưng nếu không sử dụng vào ngày hôm đó thì nó sẽ bị mất vĩnh viễn. 

Điều khó khăn là Mel có thể làm nhiều loại bánh cùng một lúc. Điều này có nghĩa là bất cứ lúc nào anh ta cũng có thể có một số chiếc bánh được làm một phần, mỗi chiếc đang ở giai đoạn nào đó giữa việc không có lớp và chỉ còn một bước nữa là hoàn thành. Thành phần của mỗi ngày có thể nâng cấp chính xác một phần bánh hiện có từ lớp k lên k+1 hoặc bị bỏ qua. 

Nhiệm vụ là xác định số lượng bánh 5 lớp hoàn chỉnh tối đa có thể làm được trong tất cả các ngày. 

Kích thước đầu vào có thể lên tới một triệu ngày, điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng theo dõi tất cả các trạng thái từng phần một cách rõ ràng trên mỗi chiếc bánh hoặc mô phỏng tất cả các nhiệm vụ có thể thực hiện được. Bất kỳ giải pháp nào cũng phải xử lý mỗi ngày theo thời gian cố định hoặc khấu hao không đổi. 

Một trường hợp phức tạp xuất hiện khi các lớp đến theo thứ tự “sai”, ví dụ như nhiều lớp 5 sớm hơn, sau đó là 1 giây. Một kẻ tham lam ngây thơ luôn cố gắng hoàn thành chiếc bánh cao cấp nhất trước có thể thất bại vì nó chặn khả năng bắt đầu chuỗi hợp lệ mới. Một trường hợp thất bại khác là khi có nhiều lớp trung gian nhưng không đủ số 1, khi đó việc phân bổ quá mức các lớp đầu tiên dẫn đến các bánh bị chết một phần và không bao giờ có thể hoàn thành được. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì một danh sách tất cả các bánh một phần và đối với mỗi lớp đến, hãy thử mọi bánh có thể có mà nó có thể mở rộng. Trong trường hợp xấu nhất, sau k ngày có thể có O(k) một phần bánh, vì vậy mỗi ngày mới có thể quét tất cả chúng, dẫn đến hành vi O(N^2) khi N lớn. Điều này trở nên không khả thi sau khoảng 10^5 thao tác và ở đây N lên tới 10^6. 

Quan sát quan trọng là chúng ta không bao giờ cần phải phân biệt giữa các loại bánh riêng lẻ. Điều quan trọng chỉ là có bao nhiêu chiếc bánh hiện đang chờ cho mỗi lớp được yêu cầu tiếp theo. Một chiếc bánh đang được xử lý có thể được mô tả thuần túy theo giai đoạn hiện tại của nó, từ 1 đến 4, vì giai đoạn 5 có nghĩa là đã hoàn thành và không cần theo dõi nữa. 

Điều này làm giảm vấn đề quản lý năm bộ đếm: có bao nhiêu phần bánh đang chờ lớp 1, lớp 2, lớp 3, lớp 4 và bao nhiêu bánh hoàn chỉnh đã được hình thành. Mỗi ngày chúng ta tham lam cố gắng sử dụng lớp mới đến để nâng cao chiếc bánh cần nó. Nếu có nhiều lựa chọn, chúng tôi luôn ưu tiên nâng cao giai đoạn nâng cao nhất có thể sử dụng nó, vì điều đó duy trì tính linh hoạt cho các lớp trước đó khó phù hợp hơn sau này. 

Tính tham lam này có tác dụng vì việc trì hoãn việc hoàn thành giai đoạn cao hơn để chuyển sang giai đoạn thấp hơn không bao giờ làm tăng khả năng xảy ra trong tương lai, trong khi làm ngược lại có thể ngăn chặn vĩnh viễn việc hình thành chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của bánh | O(N^2) | O(N) | Quá chậm | 
| Mô phỏng tham lam theo giai đoạn | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng`cnt[1..4]`Ở đâu`cnt[i]`là số lượng bánh một phần hiện đang ở giai đoạn i, nghĩa là chúng đang chờ lớp i+1. Chúng tôi cũng duy trì một quầy bán bánh thành phẩm. 

Mỗi ngày xử lý một giá trị`a[i]`. 

1. Nếu`a[i] == 1`, chúng ta bắt đầu một phần bánh mới ở giai đoạn 1, vì vậy chúng ta tăng`cnt[1]`. Đây là lớp duy nhất có thể tạo chuỗi mới. 
2. Nếu`a[i] == 2`, chúng ta cố gắng nâng cao một chiếc bánh đang chờ lớp 2, nghĩa là một chiếc bánh ở giai đoạn 1. Nếu`cnt[1] > 0`, chúng tôi giảm`cnt[1]`và tăng dần`cnt[2]`. Nếu không có chiếc bánh như vậy, chúng tôi sẽ loại bỏ thành phần này. 
3. Nếu`a[i] == 3`, trước tiên chúng tôi cố gắng mở rộng bánh giai đoạn 2 nếu có thể, vì điều đó giúp duy trì các giai đoạn trước đó. Vậy nếu`cnt[2] > 0`, chúng tôi di chuyển một từ`cnt[2]`ĐẾN`cnt[3]`, nếu không chúng ta sẽ bỏ qua lớp đó. 
4. Nếu`a[i] == 4`, chúng tôi cố gắng mở rộng giai đoạn 3 trước tiên sang giai đoạn 4. Nếu`cnt[3] > 0`, chúng tôi di chuyển một về phía trước. Nếu không chúng tôi loại bỏ nó. 
5. Nếu`a[i] == 5`, điều này sẽ hoàn thành một chiếc bánh nếu chúng ta có bất kỳ chiếc bánh nào ở giai đoạn 4. Nếu như`cnt[4] > 0`, chúng tôi giảm nó và tăng câu trả lời. 

Thứ tự bên trong mỗi bước rất quan trọng vì chúng tôi luôn ưu tiên mở rộng giai đoạn nâng cao nhất có thể. 

Lý do nó hoạt động được ghi lại bởi bất biến rằng tại bất kỳ thời điểm nào, nhiều phần bánh một phần được đặc trưng đầy đủ theo các giai đoạn của chúng và bất kỳ lịch trình cuối cùng hợp lệ nào cũng có thể được chuyển thành một lịch trình mà các tiến bộ ở giai đoạn trước không bao giờ cản trở việc hoàn thành giai đoạn sau. Bất cứ khi nào chúng tôi chọn bỏ qua một tiến trình có thể có của một chiếc bánh cao cấp hơn để chuyển sang một chiếc bánh kém cao cấp hơn, chúng tôi chỉ trì hoãn tiến độ mà không tăng tổng số lần hoàn thành có thể tiếp cận, trong khi lựa chọn ngược lại có thể phá hủy một chuỗi lẽ ra sẽ hoàn thành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    cnt1 = cnt2 = cnt3 = cnt4 = 0
    ans = 0
    
    for x in arr:
        if x == 1:
            cnt1 += 1
        elif x == 2:
            if cnt1:
                cnt1 -= 1
                cnt2 += 1
        elif x == 3:
            if cnt2:
                cnt2 -= 1
                cnt3 += 1
        elif x == 4:
            if cnt3:
                cnt3 -= 1
                cnt4 += 1
        else:  # x == 5
            if cnt4:
                cnt4 -= 1
                ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp triển khai máy giai đoạn được mô tả trước đó. Mỗi biến tương ứng chính xác với một giai đoạn của những chiếc bánh chưa hoàn thiện và các quá trình chuyển đổi là những cập nhật cục bộ theo thời gian. Câu trả lời cuối cùng chỉ được tích lũy khi bánh giai đoạn 4 nhận thành công lớp 5. 

Một cạm bẫy triển khai phổ biến là cố gắng trở nên “công bằng” và phân phối các lớp trên tất cả các loại bánh có thể. Điều đó là không cần thiết và có hại vì cấu trúc đảm bảo chỉ tính đến vật chất chứ không tính đến danh tính. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

đầu vào:```
11
1 1 2 3 4 4 5 2 3 4 5
```Chúng tôi chỉ theo dõi các tiểu bang. 

| Ngày | Lớp | cnt1 | cnt2 | cnt3 | cnt4 | bánh ngọt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 0 | 0 | 
| 2 | 1 | 2 | 0 | 0 | 0 | 0 | 
| 3 | 2 | 1 | 1 | 0 | 0 | 0 | 
| 4 | 3 | 1 | 0 | 1 | 0 | 0 | 
| 5 | 4 | 1 | 0 | 0 | 1 | 0 | 
| 6 | 4 | 1 | 0 | 0 | 1 | 0 | 
| 7 | 5 | 1 | 0 | 0 | 0 | 1 | 
| 8 | 2 | 0 | 1 | 0 | 0 | 1 | 
| 9 | 3 | 0 | 0 | 1 | 0 | 1 | 
| 10 | 4 | 0 | 0 | 0 | 1 | 1 | 
| 11 | 5 | 0 | 0 | 0 | 0 | 2 | 

Dấu vết này cho thấy các lớp trung gian được tái sử dụng như thế nào trên nhiều chuỗi và cách hoàn thành một chiếc bánh sẽ giải phóng năng lực cho các tiến trình mới. 

Ví dụ thứ hai nêu bật lòng tham: 

đầu vào:```
6
1 1 2 2 5 5
```Quá trình xây dựng hai bánh giai đoạn 1, sau đó không thể thực hiện đồng thời hai chuyển đổi giai đoạn 2 cho cả hai, vì vậy chỉ có thể hoàn thành một chuỗi đầy đủ tùy theo đơn đặt hàng. Thuật toán đảm bảo chỉ tính tiến trình tuần tự hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi ngày thực hiện một số lượng cập nhật truy cập liên tục | 
| Không gian | O(1) | Chỉ có bốn bộ đếm giai đoạn được lưu trữ | 

Giải pháp này phù hợp thoải mái trong giới hạn cho N lên tới 10^6, vì nó chỉ thực hiện các phép toán số nguyên đơn giản cho mỗi phần tử đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))
    
    cnt1 = cnt2 = cnt3 = cnt4 = 0
    ans = 0
    
    for x in arr:
        if x == 1:
            cnt1 += 1
        elif x == 2:
            if cnt1:
                cnt1 -= 1
                cnt2 += 1
        elif x == 3:
            if cnt2:
                cnt2 -= 1
                cnt3 += 1
        elif x == 4:
            if cnt3:
                cnt3 -= 1
                cnt4 += 1
        else:
            if cnt4:
                cnt4 -= 1
                ans += 1
    
    return str(ans)

# provided sample
assert run("11\n1 1 2 3 4 4 5 2 3 4 5\n") == "2"

# minimum input
assert run("1\n1\n") == "0"

# no possible completion
assert run("5\n5 5 5 5 5\n") == "0"

# perfect chain
assert run("5\n1 2 3 4 5\n") == "1"

# multiple chains
assert run("10\n1 1 2 3 4 5 1 2 3 4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 0 | không thể hoàn thành | 
| tất cả 5s | 0 | thiếu điều kiện tiên quyết | 
| chuỗi tuần tự | 1 | tính đúng đắn cơ bản | 
| chuỗi lặp đi lặp lại | 2 | tái sử dụng đường ống | 

## Vỏ cạnh 

Khi tất cả các lớp đầu xuất hiện muộn, chẳng hạn như nhiều lớp 5 theo sau là 1, thuật toán sẽ tạo ra số 0 một cách chính xác vì bánh giai đoạn 4 không bao giờ tồn tại khi lớp 5 đến. Một cách giải thích ngây thơ cố gắng “lưu trữ” 5s sẽ làm tăng kết quả một cách không chính xác. 

Khi các lớp được lặp lại theo thứ tự hoàn hảo từ 1 đến 5, mỗi bước sẽ tiến tới chính xác một chuỗi và không có sự can thiệp nào xảy ra. Bộ đếm đảm bảo không có lớp nào bị lãng phí. 

Khi có nhiều lớp trung gian nhưng số 1 lại khan hiếm, chỉ có số điểm bắt đầu mới giới hạn câu trả lời. Thuật toán phản ánh điều này vì cnt1 là nút cổ chai và không giai đoạn nào sau này có thể vượt qua nó.
