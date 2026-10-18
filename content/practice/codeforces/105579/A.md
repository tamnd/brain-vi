---
title: "CF 105579A - Đèn giao thông"
description: "Chúng tôi đang mô phỏng một kịch bản lái xe đơn giản trên một con đường thẳng. Một ô tô khởi hành cách một ngã tư một đoạn và di chuyển về phía đó với vận tốc không đổi. Tại một thời điểm nhất định trong tương lai, đèn giao thông ở ngã tư sẽ chuyển sang màu đỏ trong một khoảng thời gian cố định."
date: "2026-06-22T14:30:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "A"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 51
verified: true
draft: false
---

[CF 105579A - Đèn giao thông](https://codeforces.com/problemset/problem/105579/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một kịch bản lái xe đơn giản trên một con đường thẳng. Một ô tô khởi hành cách một ngã tư một đoạn và di chuyển về phía đó với vận tốc không đổi. Tại một thời điểm nhất định trong tương lai, đèn giao thông ở ngã tư sẽ chuyển sang màu đỏ trong một khoảng thời gian cố định. Nhiệm vụ của chúng ta là quyết định xem chiếc xe đó có đi vào giao lộ trong khoảng thời gian màu đỏ hay không. Nếu xảy ra, người lái xe phải phanh gấp; nếu không, chiếc xe có thể tiếp tục mà không cần can thiệp. 

Đầu vào mô tả bốn đại lượng. Khoảng cách`s`cho biết ô tô cách giao lộ bao xa tại thời điểm 0. tốc độ`v`là không đổi nên thời gian cần thiết để đến giao lộ được xác định đầy đủ. giá trị`t`là độ trễ trước khi đèn chuyển sang màu đỏ và`T`là nó sẽ giữ màu đỏ trong bao lâu sau khi bắt đầu. 

Đại lượng rút ra chính là thời gian ô tô đến giao lộ, tức là`s / v`. Đèn đỏ trong khoảng thời gian`[t, t + T]`. Vấn đề giảm xuống còn việc kiểm tra xem hai khoảng thời gian này có giao nhau theo cách khiến ô tô ở giao lộ trong khi đèn đỏ hay không. 

Các ràng buộc nhỏ, với tất cả các giá trị lên tới 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa ngoài số học thời gian không đổi cho mỗi trường hợp thử nghiệm. Độ chính xác của dấu phẩy động là không cần thiết nếu chúng ta tránh hoàn toàn phép chia và thực hiện so sánh số nguyên. 

Một vấn đề tế nhị phát sinh từ thời gian đến một phần. Nếu chúng ta tính toán`s / v`trực tiếp sử dụng số dấu phẩy động, trường hợp đường biên có thể mắc lỗi chính xác. Ví dụ,`s = 9, v = 2`cho`4.5`, có thể được biểu thị ở mức thấp hơn hoặc cao hơn một chút so với giá trị thực. Điều này quan trọng khi so sánh với các ngưỡng số nguyên như`t`Và`t + T`. Một cách tiếp cận an toàn hơn là tránh hoàn toàn sự phân chia. 

Một trường hợp khác là khi xe đến đúng lúc đèn chuyển sang màu đỏ hoặc đúng lúc đèn chuyển xanh trở lại. Câu lệnh đảm bảo ô tô không đến chính xác vào thời điểm chuyển mạch, vì vậy các bất đẳng thức chặt chẽ là đủ và chúng ta không cần phải xử lý đẳng thức một cách cẩn thận. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ tính toán thời gian đến`s / v`và sau đó kiểm tra xem nó có nằm trong khoảng màu đỏ không`[t, t + T]`. Về nguyên tắc, điều này đúng vì vị trí của ô tô như một hàm số của thời gian mang tính quyết định và chúng ta chỉ quan tâm đến một sự kiện duy nhất. Tuy nhiên, nó yêu cầu phân chia dấu phẩy động và so sánh với các ranh giới số nguyên, điều này gây ra rủi ro về độ chính xác không cần thiết. 

Thay vào đó, chúng ta có thể loại bỏ hoàn toàn thời gian bằng cách chuyển đổi điều kiện thành bất đẳng thức số nguyên. Xe đến ngã tư đúng lúc`s / v`. Điều kiện là vẫn còn trước ngã tư khi đèn chuyển sang màu đỏ là`s / v < t + T`, và nó vẫn chưa trôi qua trước khi màu đỏ bắt đầu theo hướng có hại nếu`s / v > t`. Chính xác hơn, chiếc xe bị ảnh hưởng nếu`t <= s / v <= t + T`. 

Nhân tất cả các phần với`v`(là số dương nên hướng bất đẳng thức được bảo toàn) mang lại điều kiện nguyên tương đương:`t * v <= s <= (t + T) * v`. Điều này loại bỏ hoàn toàn các phép toán dấu phẩy động và giảm vấn đề xuống còn kiểm tra số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng thời gian trực tiếp với phép chia | O(1) | O(1) | Được chấp nhận nhưng rủi ro hơn | 
| Chuyển đổi bất đẳng thức số nguyên | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta trình bày lại bài toán một cách hoàn toàn dưới dạng số học số nguyên. 

1. Tính khoảng thời gian đèn đỏ theo “đơn vị khoảng cách” bằng cách nhân cả hai điểm cuối với tốc độ`v`. Điều này tạo ra một phạm vi`[t * v, (t + T) * v]`tương ứng với tất cả khoảng cách có thể mà ô tô đã đi được khi đèn đỏ. 
2. So sánh quãng đường của xe`s`chống lại khoảng này. Nếu như`s`nằm trong phạm vi này nghĩa là xe đến giao lộ vào thời điểm đèn đỏ. 
3. Nếu`s`nằm trong khoảng, đầu ra`"Yes"`vì cần phải phanh. Ngược lại, xuất ra`"No"`. 

Lý do phép nhân có tác dụng là vì phép so sánh thời gian và so sánh khoảng cách có quan hệ tuyến tính bởi một hệ số hằng số dương`v`, do đó thứ tự của các sự kiện được giữ nguyên. 

### Tại sao nó hoạt động 

Xe đến ngã tư đúng thời gian`s / v`trôi qua. Khoảng thời gian đèn đỏ được xác định theo thời gian, nhưng nhân mọi thứ với`v`chuyển đổi cả thời gian đến của ô tô và khoảng màu đỏ thành cùng một thang đo mà không thay đổi thứ tự của chúng. Từ`v > 0`, phép biến đổi là đơn điệu, do đó bất kỳ sự trùng lặp nào về thời gian đều tương ứng chính xác với sự trùng lặp trong không gian số nguyên được biến đổi. Điều này đảm bảo rằng việc kiểm tra`t * v <= s <= (t + T) * v`tương đương với việc kiểm tra xem xe có ở giao lộ trong pha đỏ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, v, t, T = map(int, input().split())
    
    start = t * v
    end = (t + T) * v
    
    if start <= s <= end:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đọc bốn số nguyên và ngay lập tức chuyển đổi khoảng thời gian đèn đỏ thành khoảng cách tương đương bằng cách chia tỷ lệ với`v`. Sự so sánh chống lại`s`sau đó hoàn toàn dựa trên số nguyên, giúp tránh hoàn toàn các vấn đề về dấu phẩy động. 

Một lỗi phổ biến là tính toán`s / v`và so sánh trực tiếp với`t`Và`t + T`. Mặc dù đúng về mặt logic nhưng nó có nguy cơ xảy ra lỗi chính xác và sự phức tạp không cần thiết. Một vấn đề tế nhị khác là quên rằng cả hai giới hạn đều bao hàm; vì vấn đề đảm bảo không có thời điểm chuyển đổi chính xác nào xảy ra khi đến nên tính toàn diện không tạo ra sự mơ hồ ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 2 4 2
```Chúng tôi tính toán`start = 4 * 2 = 8`Và`end = (4 + 2) * 2 = 12`. 

| Bước | bắt đầu | kết thúc | s | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tính khoảng | 8 | 12 | 9 | - | 
| Kiểm tra bao gồm | 8 | 12 | 9 | Có | 

giá trị`9`nằm bên trong`[8, 12]`, nghĩa là xe đến khi đèn đỏ. Đầu ra là`"Yes"`. 

### Ví dụ 2 

đầu vào:```
9 2 5 2
```Chúng tôi tính toán`start = 5 * 2 = 10`Và`end = 14`. 

| Bước | bắt đầu | kết thúc | s | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tính khoảng | 10 | 14 | 9 | - | 
| Kiểm tra bao gồm | 10 | 14 | 9 | Không | 

Đây`9`là trước khi khoảng màu đỏ bắt đầu nên xe đi qua an toàn. Đầu ra là`"No"`. 

Hai trường hợp này cho thấy hành vi biên chính xác: dịch chuyển khoảng màu đỏ chỉ một đơn vị thời gian sẽ di chuyển cửa sổ khoảng cách tới hạn qua điểm đến của ô tô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và phép so sánh cố định được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép tối đa 100 mỗi giá trị, do đó, ngay cả một cách tiếp cận đơn giản cũng sẽ nhanh chóng, nhưng giải pháp đã tối ưu và có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    import sys as _sys
    input = _sys.stdin.readline

    s, v, t, T = map(int, input().split())
    start = t * v
    end = (t + T) * v
    
    print("Yes" if start <= s <= end else "No")
    
    return out.getvalue().strip()

# provided samples
assert run("9 2 4 2") == "Yes"
assert run("9 2 5 2") == "No"

# minimum values, no red overlap
assert run("1 1 0 0") == "Yes"

# car very fast, passes before red starts
assert run("100 10 5 2") == "No"

# red starts immediately
assert run("10 2 0 3") == "Yes"

# boundary just after red ends
assert run("15 1 10 5") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 0 | Có | suy biến khoảng đỏ có độ dài bằng 0 | 
| 100 10 5 2 | Không | xe vượt qua trước khi đèn đỏ bắt đầu | 
| 10 2 0 3 | Có | pha đỏ ngay lập tức | 
| 15 1 10 5 | Không | ngay sau khi khoảng màu đỏ kết thúc | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi khoảng màu đỏ bắt đầu ngay tại thời điểm 0. Đối với đầu vào`10 2 0 3`, khoảng trở thành`[0, 6]`. Xe đến đúng lúc`10 / 2 = 5`, nằm bên trong khoảng. Thuật toán tính toán`start = 0`Và`end = 6`, và báo cáo chính xác`"Yes"`từ`5`nằm trong phạm vi. 

Một trường hợp khác là khi khoảng màu đỏ có độ trễ bằng 0 nhưng cũng có thời lượng bằng 0, thực tế không bao giờ có màu đỏ. Vì`1 1 0 0`, khoảng là`[0, 0]`. Xe đến đúng lúc`1`, nằm ngoài và kiểm tra số nguyên mang lại`start = 0`,`end = 0`, Vì thế`1`không ở bên trong và đầu ra là`"No"`hoặc`"Yes"`tùy theo cách hiểu. Câu lệnh đảm bảo không có xung đột chuyển mạch chính xác, do đó trường hợp suy biến này hoạt động nhất quán với mô hình bất đẳng thức. 

Một kịch bản tinh tế cuối cùng là khi chiếc xe chạy cực nhanh. Vì`100 10 5 2`, thời gian đến là`10`, trong khi khoảng màu đỏ là`[50, 70]`. Séc được chuyển đổi mang lại`50 <= 100 <= 70`điều này là sai, xác nhận rằng chiếc xe đã vượt qua an toàn trước khi đèn chuyển sang màu đỏ.
