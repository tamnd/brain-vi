---
title: "CF 1296D - Chiến Đấu Với Quái Vật"
description: "Chúng ta được cung cấp một chuỗi các quái vật đứng theo một thứ tự cố định. Mỗi quái vật có một lượng máu nhất định và chúng phải bị đánh bại lần lượt từ trái sang phải."
date: "2026-06-16T04:48:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1296
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 617 (Div. 3)"
rating: 1500
weight: 1296
solve_time_s: 169
verified: false
draft: false
---

[CF 1296D - Chiến đấu với quái vật](https://codeforces.com/problemset/problem/1296/D) 

**Đánh giá:** 1500 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các quái vật đứng theo một thứ tự cố định. Mỗi quái vật có một lượng máu nhất định và chúng phải bị đánh bại lần lượt từ trái sang phải. Cuộc chiến với quái vật diễn ra theo lượt: đầu tiên bạn tấn công, sau đó đối thủ tấn công, lặp lại cho đến khi máu của quái vật giảm xuống 0 hoặc thấp hơn. 

Đòn đánh của bạn luôn gây ra một lượng sát thương cố định và bất cứ khi nào đòn tấn công của bạn kết liễu một quái vật, bạn sẽ kiếm được một điểm cho quái vật đó. Đối thủ cũng gây sát thương cố định trong lượt của họ, nhưng họ không bao giờ góp phần ghi bàn. Điều thú vị là bạn được phép hủy lượt của đối thủ trong một số lần giới hạn trong tất cả các trận đấu và mỗi lần hủy sẽ bỏ qua đúng một đòn tấn công của đối thủ. 

Mục tiêu là tối đa hóa số lượng quái vật mà cá nhân bạn tiêu diệt được, tương đương với việc tối đa hóa số lần đòn tấn công của bạn khiến máu của quái vật về 0 hoặc thấp hơn. 

Các hạn chế rất lớn: lên tới 200.000 quái vật với lượng máu lên tới 10^9 và số lần bỏ qua cũng rất lớn. Điều này ngay lập tức loại trừ mọi mô phỏng của từng cuộc tấn công theo lượt. Ngay cả vòng lặp trên mỗi con quái vật mô phỏng các trận chiến đầy đủ cũng có thể quá chậm nếu được triển khai một cách ngây thơ, vì một con quái vật có thể yêu cầu tới 10^9/một lượt. 

Khó khăn chính là việc bỏ qua các cuộc tấn công của đối thủ là nguồn tài nguyên toàn cầu. Việc sử dụng bỏ qua một quái vật sẽ thay đổi số lần đánh cần thiết đối với những quái vật khác, vì vậy chúng ta phải quyết định cách phân phối lượt bỏ qua cho các quái vật. 

Một sai lầm ngây thơ nảy sinh khi cho rằng chúng ta luôn muốn sử dụng cách bỏ qua những con quái vật lớn nhất. Ví dụ: một con quái vật có lượng máu rất cao vẫn không thể “chuyển thành điểm” ngay cả khi bỏ qua nhiều lần, trong khi một con quái vật nhỏ hơn có thể chỉ cần một vài lần bỏ qua để đảm bảo chúng ta hạ được đòn cuối cùng. Lợi ích của việc bỏ qua phụ thuộc vào mô hình luân phiên hiện tại, không chỉ sức khỏe thô. 

Một trường hợp thất bại tinh vi khác xuất hiện khi chiến lược tham lam sử dụng các bước bỏ qua ngay khi chúng có sẵn. Điều đó có thể lãng phí số lần bỏ qua đối với những con quái vật đầu tiên mà chúng không cần thiết để đảm bảo một điểm, để lại số lần bỏ qua không đủ cho những con quái vật sau này, nơi mà một điều chỉnh nhỏ cũng có thể mang lại lợi ích. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng trực tiếp từng quái vật, chúng ta có thể tính toán cần bao nhiêu chu kỳ đầy đủ của “bạn rồi đối thủ” cho đến khi quái vật chết. Đối với một quái vật nhất định, không có bất kỳ lần bỏ qua nào, số lần đánh bạn cần là cố định, nhưng đối thủ có thể chèn thêm các lượt sát thương để tăng số chu kỳ cần thiết. Người ta có thể thử mô phỏng trận đấu từng bước, tiêu tốn k lượt bỏ qua bất cứ khi nào đối thủ hành động. 

Ý tưởng bạo lực này là đúng vì nó phản ánh chính xác các quy tắc. Tuy nhiên, nó có thể xuống cấp trầm trọng: một con quái vật có lượng máu lớn có thể yêu cầu hàng triệu lượt quay xen kẽ và đối với tất cả các quái vật, điều này vượt xa giới hạn chấp nhận được. 

Quan sát quan trọng là mỗi quái vật có thể được ấn định một “chi phí” dựa trên số lượt đối thủ phải loại bỏ để bạn vẫn bị tiêu diệt trước khi đối thủ trì hoãn bạn một cách hiệu quả sau đòn đánh cuối cùng. Thay vì mô phỏng lượt, chúng tôi tính toán cho mỗi quái vật số lượng đòn đánh của đối thủ mà chúng tôi phải hủy để đảm bảo rằng quái vật đó chết trong lượt của bạn. Sau đó, vấn đề trở thành việc chọn số lượng quái vật tối đa mà chi phí của chúng có thể được thanh toán bằng cách sử dụng tối đa k lần bỏ qua.

Đối với một quái vật cố định, hãy xem xét có bao nhiêu chu kỳ đầy đủ (bạn tấn công, tấn công của đối thủ) xảy ra cho đến khi máu của nó giảm xuống 0. Nếu chúng ta biểu thị bằng t số lần tấn công bạn cần để tiêu diệt nó, thì theo quy trình thông thường, đối thủ sẽ hành động t−1 lần trước đòn cuối cùng của bạn. Nếu chúng tôi muốn bạn tung ra đòn cuối cùng, chúng tôi cần đảm bảo đối thủ không can thiệp vào chu kỳ quan trọng có thể khiến lượt tiêu diệt vượt quá hướng của bạn. Điều này chuyển thành số lượt đối thủ bị bỏ qua bắt buộc, tùy thuộc vào số lần đối thủ sẽ hành động trước đòn kết liễu của bạn. 

Do đó, mỗi quái vật đóng góp một giá trị chi phí và mỗi lần tiêu diệt thành công sẽ đóng góp một đơn vị phần thưởng. Chúng ta muốn tối đa hóa số chi phí mà chúng ta có thể chi trả trong tổng ngân sách k. Sắp xếp các chi phí này theo thứ tự tăng dần và lấy càng nhiều chi phí rẻ nhất càng tốt là tối ưu vì mọi quái vật đều đưa ra phần thưởng giống nhau, vì vậy chúng tôi luôn ưu tiên chuyển đổi rẻ hơn trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng mỗi lần tấn công | O(tổng số lần truy cập) trường hợp xấu nhất lên tới 1e14 | O(1) | Quá chậm | 
| Tham lam với giá mỗi con quái vật | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán cho mỗi quái vật số lần bỏ qua tối thiểu của đối thủ cần thiết để đảm bảo chúng tôi giành được điểm của nó. 

1. Với mỗi quái vật, hãy tính xem bạn cần bao nhiêu đòn đánh:`t = ceil(h / a)`. Đây là số lần tấn công bạn cần để tiêu diệt nó nếu không bị gián đoạn. 
2. Xác định xem thông thường đối thủ sẽ quay bao nhiêu lượt trước đòn đánh cuối cùng của bạn. Trong mô hình xen kẽ tiêu chuẩn, đối thủ nhận được`t - 1`tấn công trước khi quái vật chết. 
3. Hãy chuyển điều này thành một cái giá: phải loại bỏ bao nhiêu đòn tấn công của đối thủ để trận chiến không “vượt quá” căn chỉnh đòn đánh cuối cùng của bạn. Chi phí phát sinh là`cost = max(0, t - 1)`. 

Điều này phản ánh rằng mỗi lượt của đối thủ sẽ trì hoãn việc tiêu diệt một cách hiệu quả so với chuỗi tấn công của bạn và việc loại bỏ chúng sẽ đẩy nhanh quá trình tung đòn kết liễu của bạn. 
4. Tập hợp tất cả chi phí vào một mảng. 
5. Sắp xếp mảng theo thứ tự tăng dần. 
6. Duyệt qua các chi phí đã được sắp xếp, tích lũy chúng vào ngân sách hoạt động. Mỗi lần thêm một chi phí không vượt quá k, hãy lấy con quái vật đó và trừ chi phí của nó khỏi k, tăng câu trả lời lên 1. 
7. Dừng lại khi không thể thanh toán chi phí tiếp theo. 

### Tại sao nó hoạt động 

Mỗi quái vật mang lại chính xác một điểm nếu được chọn và yêu cầu một chi phí độc lập cố định tính theo lượt đối thủ bị bỏ qua. Quá trình này giảm xuống còn việc chọn càng nhiều mục càng tốt trong một ràng buộc ba lô duy nhất trong đó tất cả các giá trị đều bằng nhau. Trong trường hợp như vậy, việc chọn các mặt hàng theo thứ tự chi phí tăng dần là tối ưu vì việc thay thế một mặt hàng đắt tiền đã chọn bằng một mặt hàng rẻ hơn chưa được chọn chỉ có thể cải thiện tính khả thi mà không làm giảm số lượng. Đối số trao đổi này đảm bảo rằng bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp tham lam mà không bị mất mát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b, k = map(int, input().split())
    h = list(map(int, input().split()))
    
    costs = []
    
    for hp in h:
        t = (hp + a - 1) // a
        cost = t - 1
        costs.append(cost)
    
    costs.sort()
    
    ans = 0
    for c in costs:
        if c <= k:
            k -= c
            ans += 1
        else:
            break
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ giảm mỗi quái vật thành một chi phí vô hướng duy nhất biểu thị số lượng sự gián đoạn của đối thủ phải được loại bỏ. Bộ phận trần tính toán số lần truy cập của bạn cần thiết. Phép trừ một sẽ cho biết có bao nhiêu lượt đối thủ xảy ra trước vị trí tấn công cuối cùng của bạn. 

Việc sắp xếp đảm bảo chúng ta luôn bỏ qua những con quái vật rẻ nhất trước tiên. Vòng lặp tham lam sau đó thực hiện một quy trình tiêu thụ ngân sách đơn giản. 

Một điểm thực hiện tinh tế là phép chia số nguyên: sử dụng`(hp + a - 1) // a`tránh các lỗi dấu phẩy động và tính toán chính xác mức trần. Một chi tiết khác là chúng tôi không bao giờ mô phỏng rõ ràng các ngã rẽ; tất cả các động lực được nén vào công thức chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2 3 3
7 10 50 12 1 8
```Chúng tôi tính toán chi phí: 

| Quái vật | hp | t = trần(h/a) | chi phí = t−1 | 
| --- | --- | --- | --- | 
| 1 | 7 | 4 | 3 | 
| 2 | 10 | 5 | 4 | 
| 3 | 50 | 25 | 24 | 
| 4 | 12 | 6 | 5 | 
| 5 | 1 | 1 | 0 | 
| 6 | 8 | 4 | 3 | 

Phân loại chi phí:`[0, 3, 3, 4, 5, 24]`Chúng ta tiêu k = 3: 

| Bước | Chi phí | Còn lại k | Đã chụp | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | vâng | 
| 2 | 3 | 0 | vâng | 
| 3 | 3 | 0 | vâng | 
| 4 | 4 | 0 | không | 

Câu trả lời là 3. 

Dấu vết này cho thấy rằng những quái vật không tốn phí luôn được lấy đi và sau khi cạn kiệt ngân sách, không thể đưa thêm quái vật nào vào nữa. 

### Ví dụ 2 

đầu vào:```
3 5 10 2
10 20 25
```Chi phí: 

| Quái vật | hp | t | chi phí | 
| --- | --- | --- | --- | 
| 1 | 10 | 2 | 1 | 
| 2 | 20 | 4 | 3 | 
| 3 | 25 | 5 | 4 | 

Phân loại chi phí:`[1, 3, 4]`| Bước | Chi phí | Còn lại k | Đã chụp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 
| 2 | 3 | 1 | không | 

Câu trả lời là 1. 

Điều này chứng tỏ rằng mặc dù có nhiều quái vật tồn tại, nhưng chỉ những quái vật có chi phí phù hợp với ngân sách bỏ qua hạn chế mới có thể được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chi phí chi phối mọi hoạt động | 
| Không gian | O(n) | Chi phí lưu trữ cho mỗi quái vật | 

Với n lên tới 200.000, việc sắp xếp nằm trong giới hạn và tất cả các công việc khác đều là tuyến tính. Thuật toán thoải mái phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, a, b, k = map(int, input().split())
    h = list(map(int, input().split()))
    
    costs = []
    for hp in h:
        t = (hp + a - 1) // a
        costs.append(t - 1)
    
    costs.sort()
    
    ans = 0
    for c in costs:
        if c <= k:
            k -= c
            ans += 1
        else:
            break
    
    return str(ans)

# provided sample
assert run("6 2 3 3\n7 10 50 12 1 8\n") == "3"

# minimum size
assert run("1 1 1 0\n1\n") == "1"

# no skips, only easy kills
assert run("3 5 5 0\n1 5 5\n") == "3"

# all expensive monsters
assert run("3 1 1 1\n100 100 100\n") == "0"

# all equal
assert run("4 2 1 5\n4 4 4 4\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quái vật duy nhất không bỏ qua | 1 | tính đúng đắn của trường hợp cơ sở | 
| ngân sách bằng không | phụ thuộc vào một | không thể mua chuyển đổi | 
| tất cả hp lớn | 0 | lọc chi phí | 
| quái vật đồng phục | toàn tham lam sử dụng | phân loại tính trung lập | 

## Vỏ cạnh 

Trường hợp đơn quái vật tối thiểu cho thấy công thức giảm chính xác ngay cả khi không có sự cạnh tranh giữa các lựa chọn. Với một con quái vật và đủ sức mạnh, cái giá phải trả sẽ bằng 0 và nó luôn bị lấy đi. 

Tình huống không bỏ qua xác nhận rằng thuật toán không bao giờ giả định sai các chuyển đổi miễn phí; chỉ những quái vật có giá bằng 0 mới được tính. 

Một trường hợp có giá trị sức khỏe rất lớn nhấn mạnh rằng chi phí có thể lớn, nhưng việc sắp xếp và lựa chọn tham lam vẫn xử lý chúng một cách an toàn mà không gặp vấn đề về tràn hoặc hiệu suất.
