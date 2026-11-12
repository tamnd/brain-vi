---
title: "CF 1676A - May mắn?"
description: "Chúng ta được cung cấp một chuỗi các chuỗi ngắn, mỗi chuỗi đại diện cho một số vé có sáu chữ số. Mỗi vé nên được chia thành hai nửa có chiều dài bằng nhau. Nhiệm vụ là quyết định xem tổng các chữ số ở nửa bên trái có khớp với tổng các chữ số ở nửa bên phải hay không."
date: "2026-06-10T00:57:36+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1676
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 790 (Div. 4)"
rating: 800
weight: 1676
solve_time_s: 95
verified: true
draft: false
---

[CF 1676A - May mắn?](https://codeforces.com/problemset/problem/1676/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chuỗi ngắn, mỗi chuỗi đại diện cho một số vé có sáu chữ số. Mỗi vé nên được chia thành hai nửa có chiều dài bằng nhau. Nhiệm vụ là quyết định xem tổng các chữ số ở nửa bên trái có khớp với tổng các chữ số ở nửa bên phải hay không. 

Mỗi trường hợp kiểm thử là độc lập nên việc tính toán sẽ được đặt lại cho mỗi chuỗi yêu cầu. Đầu ra là một phán đoán nhị phân đơn giản cho mỗi vé: liệu nó có thỏa mãn điều kiện cân bằng này hay không. 

Mặc dù định dạng đầu vào cực kỳ nhỏ nhưng điều chính cần cẩn thận là các chữ số được đưa ra dưới dạng ký tự. Việc xử lý chúng dưới dạng số nguyên yêu cầu chuyển đổi rõ ràng, nếu không phép so sánh hoặc số học sẽ hoạt động không chính xác trong âm thầm. 

Các ràng buộc đủ chặt chẽ để bất kỳ cách giải thích hợp lý nào cũng đủ. Với tối đa 1000 vé và chỉ sáu ký tự trên mỗi vé, ngay cả một giải pháp thực hiện công việc dư thừa cho mỗi trường hợp thử nghiệm vẫn sẽ chạy thoải mái trong giới hạn. Điều này đẩy vấn đề một cách chắc chắn vào hạng mục triển khai, trong đó tính chính xác đến từ việc lập chỉ mục và chuyển đổi cẩn thận thay vì tối ưu hóa thuật toán. 

Trường hợp cạnh phổ biến nhất ở đây là quên rằng chuỗi có thể chứa các số 0 đứng đầu. Một tấm vé như`"045207"`phải được xử lý chính xác bằng sáu chữ số, không phải là số mất các số 0 đứng đầu. Một lỗi nhỏ khác là vô tình tính tổng các giá trị ASCII thay vì các giá trị chữ số, điều này xảy ra nếu người ta quên trừ`'0'`. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là tách chuỗi thành hai phần một cách rõ ràng và tính tổng của mỗi bên. Đối với mỗi vé, chúng tôi lặp lại ba ký tự đầu tiên, chuyển đổi chúng thành số nguyên và tính tổng chúng. Sau đó, chúng tôi lặp lại quy trình tương tự cho ba ký tự cuối cùng. Cuối cùng, chúng tôi so sánh hai kết quả. 

Cách tiếp cận này đã tối ưu trong cài đặt này. Không có cấu trúc nào để khai thác ngoài việc tổng hợp trực tiếp vì kích thước đầu vào cho mỗi lần kiểm tra là không đổi. Bất kỳ phương pháp thay thế nào vẫn cần phải đọc tất cả sáu chữ số ít nhất một lần, điều đó có nghĩa là chúng tôi không thể cải thiện một cách tiệm cận công việc tuyến tính trên mỗi vé. 

Quan sát chính là vấn đề không yêu cầu bất kỳ lý luận hoặc tiền xử lý tổng thể nào. Mỗi vé là độc lập và việc tính toán có kích thước cố định. Vì vậy, giải pháp giảm xuống việc xử lý ký tự đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tổng trực tiếp một nửa) | O(6t) | O(1) | Đã chấp nhận | 
| Tối ưu (giống nhau, thực hiện trực tiếp) | O(6t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng vé một cách độc lập và tính toán hai khoản tiền. 

1. Đọc chuỗi vé. Nó luôn có độ dài 6, vì vậy chúng ta có thể lập chỉ mục cho nó một cách an toàn mà không cần kiểm tra giới hạn. 
2. Khởi tạo hai bộ tích lũy, một cho nửa bên trái và một cho nửa bên phải. 
3. Thêm các chữ số ở vị trí 0, 1 và 2 vào tổng bên trái sau khi chuyển từng ký tự thành số nguyên. Điều này đảm bảo chúng ta đang làm việc trong không gian số thay vì không gian mã hóa ký tự. 
4. Cộng các chữ số ở vị trí 3, 4 và 5 vào tổng bên phải theo cách tương tự. 
5. So sánh hai tổng. Nếu chúng khớp nhau, xuất ra`"YES"`, nếu không thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Mỗi vé được chia thành hai bộ chữ số riêng biệt. Vì mỗi chữ số đóng góp chính xác một lần vào tổng bên trái hoặc bên phải và không có sự trùng lặp nên việc so sánh các tổng sẽ nắm bắt trực tiếp điều kiện được mô tả trong bài toán. Không có sự phụ thuộc tiềm ẩn giữa các vị trí, do đó độ chính xác giảm xuống mức tích lũy chính xác cho mỗi vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    s = input().strip()
    
    left = 0
    right = 0
    
    left += ord(s[0]) - ord('0')
    left += ord(s[1]) - ord('0')
    left += ord(s[2]) - ord('0')
    
    right += ord(s[3]) - ord('0')
    right += ord(s[4]) - ord('0')
    right += ord(s[5]) - ord('0')
    
    if left == right:
        out.append("YES")
    else:
        out.append("NO")

print("\n".join(out))
```Giải pháp đọc mỗi phiếu dưới dạng một chuỗi thô và loại bỏ dòng mới một cách rõ ràng để tránh vô tình làm nhiễm bẩn ký tự. Mỗi chữ số được chuyển đổi bằng cách sử dụng`ord(c) - ord('0')`, đây là một chuyển đổi theo thời gian liên tục đáng tin cậy để tránh bị chậm hơn`int()`các cuộc gọi bên trong các vòng lặp chặt chẽ, mặc dù trong vấn đề này, cả hai đều ổn. 

Các tổng được tính riêng để đảm bảo rõ ràng, đảm bảo không có sự trộn lẫn các chỉ số giữa các nửa. Việc so sánh cuối cùng được thực hiện trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào từ mẫu. 

### Ví dụ 1 

Phiếu đầu vào:`213132`| Bước | Chữ số bên trái được xử lý | Chữ số bên phải được xử lý | Tổng trái | Tổng đúng | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | - | 2 | 0 | - | 
| 2 | 2,1 | - | 3 | 0 | - | 
| 3 | 2,1,3 | - | 6 | 0 | - | 
| 4 | - | 1 | 6 | 1 | - | 
| 5 | - | 1,3 | 6 | 4 | - | 
| 6 | - | 1,3,2 | 6 | 6 | CÓ | 

Điều này cho thấy việc tích lũy mang tính độc lập theo mỗi nửa và chỉ được so sánh ở cuối. 

### Ví dụ 2 

Phiếu đầu vào:`973894`| Bước | Chữ số bên trái được xử lý | Chữ số bên phải được xử lý | Tổng trái | Tổng đúng | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | - | 9 | 0 | - | 
| 2 | 9,7 | - | 16 | 0 | - | 
| 3 | 9,7,3 | - | 19 | 0 | - | 
| 4 | - | 8 | 19 | 8 | - | 
| 5 | - | 8,9 | 19 | 17 | - | 
| 6 | - | 8,9,4 | 19 | 21 | KHÔNG | 

Điều này xác nhận rằng ngay cả sự mất cân bằng nhỏ ở bất kỳ chữ số nào cũng sẽ được đưa vào so sánh cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra xử lý chính xác 6 chữ số, do đó tổng công việc tỷ lệ tuyến tính với số lượng vé | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng ngoài bộ lưu trữ đầu ra | 

Các ràng buộc cho phép tối đa 1000 vé, do đó, quá trình quét tuyến tính trên một chuỗi có kích thước không đổi này rất nhanh. Ngay cả với chi phí Python, giải pháp vẫn chạy tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        s = input().strip()
        left = sum(ord(s[i]) - 48 for i in range(3))
        right = sum(ord(s[i]) - 48 for i in range(3, 6))
        res.append("YES" if left == right else "NO")
    return "\n".join(res)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""5
213132
973894
045207
000000
055776
""") == """YES
NO
YES
YES
NO"""

# custom cases
assert run("""1
111111
""") == "YES", "all digits equal"

assert run("""1
100001
""") == "YES", "balanced with zeros"

assert run("""1
123456
""") == "NO", "strictly increasing imbalance"

assert run("""1
000999
""") == "NO", "boundary split imbalance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 111111 | CÓ | đối xứng tất cả các chữ số | 
| 100001 | CÓ | số 0 ở đầu/cuối | 
| 123456 | KHÔNG | mất cân bằng chung | 
| 000999 | KHÔNG | độ lệch cực lớn giữa hai nửa | 

## Vỏ cạnh 

Trường hợp chính đang xử lý các số 0 đứng đầu một cách chính xác. Ví dụ, trong`045207`, nửa bên trái chứa`0,4,5`, phải được xử lý bằng số như`0 + 4 + 5 = 9`. Nếu một lập trình viên vô tình chuyển đổi chuỗi con`"045"`sử dụng phân tích cú pháp số nguyên theo cách loại bỏ cấu trúc và sau đó xử lý các chữ số không chính xác, họ có thể hiểu sai hoặc xử lý sai vị trí, nhưng việc chuyển đổi trực tiếp trên mỗi ký tự sẽ tránh được hoàn toàn vấn đề đó. 

Một trường hợp tinh tế khác là vé có tất cả các chữ số bằng 0, chẳng hạn như`000000`. Hành vi đúng đắn luôn là`"YES"`vì cả hai tổng đều bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì mỗi ký tự chuyển đổi thành 0 và cả hai bộ tích lũy vẫn bằng nhau trong suốt quá trình thực thi. 

Cuối cùng, những trường hợp mất cân bằng tối đa như`999000`kiểm tra xem ranh giới phân chia có được xử lý chính xác hay không. Tổng bên trái trở thành`27`và tổng đúng sẽ trở thành`0`và vì các nửa rời rạc và được lập chỉ mục nhất quán nên việc so sánh sẽ tạo ra kết quả chính xác`"NO"`mà không có bất kỳ rủi ro nào.
