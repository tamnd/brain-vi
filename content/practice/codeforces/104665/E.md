---
title: "CF 104665E - Riddle Me This (Phiên bản dễ)"
description: "Chúng ta được cho một số chẵn các hoán vị, tất cả đều có cùng độ dài. Mỗi hoán vị đại diện cho một đối tượng tuần hoàn: chúng ta được phép xoay nó bao nhiêu lần, nghĩa là chúng ta có thể chọn bất kỳ sự dịch chuyển tuần hoàn nào của các phần tử của nó."
date: "2026-06-29T09:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104665
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 1 (Advanced)"
rating: 0
weight: 104665
solve_time_s: 75
verified: false
draft: false
---

[CF 104665E - Riddle Me This (Phiên bản dễ)](https://codeforces.com/problemset/problem/104665/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số chẵn các hoán vị, tất cả đều có cùng độ dài. Mỗi hoán vị đại diện cho một đối tượng tuần hoàn: chúng ta được phép xoay nó bao nhiêu lần, nghĩa là chúng ta có thể chọn bất kỳ sự dịch chuyển tuần hoàn nào của các phần tử của nó. Một hoán vị được coi là giải quyết được khi sau một số phép quay, nó trở thành dãy được sắp xếp từ 1 đến s. 

Điểm mấu chốt là các hoán vị không độc lập. Chúng ta phải ghép chúng lại và trong mỗi cặp, cả hai hoán vị luôn trải qua cùng một phép quay đồng thời. Vòng xoay được áp dụng cho một đối tượng sẽ tự động áp dụng cho đối tác của nó. Đối với mỗi cặp, chúng ta được phép chọn cách xoay nhưng cả hai mảng đều di chuyển cùng nhau. 

Đối với một hoán vị đơn, việc giải nó có nghĩa là tồn tại một phép quay biến nó thành hoán vị nhận dạng. Điều này tương đương với việc nói rằng hoán vị là một sự dịch chuyển theo chu kỳ của danh tính. 

Đối với một cặp, chúng tôi chọn số vòng quay, áp dụng nó cho cả hai và sau đó kiểm tra xem cái nào trong hai cái được sắp xếp. Mục đích là ghép các hoán vị sao cho tổng số hoán vị có thể giải được là lớn nhất. 

Các ràng buộc rất nhỏ: N nhiều nhất là 1000 và mỗi hoán vị có độ dài tối đa là 1000. Điều này loại trừ mọi mô phỏng bậc hai trên mỗi cặp qua các phép quay kết hợp với kết hợp nhiều giữa tất cả các cặp nếu mỗi so sánh đắt tiền. Tuy nhiên, N đủ nhỏ để có thể xây dựng O(N^2) với quá trình tiền xử lý cẩn thận. 

Một cách tiếp cận đơn giản sẽ thử tất cả các cặp và mô phỏng xem mỗi cặp có thể giải được 0, 1 hoặc 2 hoán vị hay không. Điều đó ngay lập tức trở thành giai thừa trong N, điều này không thể thực hiện được ngay cả khi N = 20. 

Một chế độ lỗi tinh vi hơn sẽ xuất hiện nếu người ta cố gắng ghép nối một cách tham lam các hoán vị trông có vẻ "dễ sửa" mà không tính đến khả năng tương thích xoay vòng được chia sẻ. Hai hoán vị có thể giải được một mình nhưng không thể giải đồng thời trong cùng một ca. 

## Phương pháp tiếp cận 

Quan sát cốt lõi là các phép quay chỉ quan trọng ở mức tương đương theo chu kỳ. Đối với mỗi hoán vị, chúng tôi quan tâm đến việc dịch chuyển nào biến nó thành hoán vị nhận dạng. Vì danh tính là cố định nên mỗi hoán vị tương ứng với một tập hợp "các ca tốt", nghĩa là các phép quay giải quyết nó. 

Nếu chúng tôi sửa một tham chiếu, giả sử chúng tôi coi góc quay 0 là vị trí nhận dạng, thì đối với mỗi hoán vị, chúng tôi có thể tính toán độ dịch chuyển duy nhất sẽ căn chỉnh phần tử đầu tiên của nó (hoặc bất kỳ điểm neo nào) thành 1 và xác minh tính nhất quán trên tất cả các vị trí. Mạnh mẽ hơn, chúng tôi tính toán tất cả các ca làm cho hoán vị bằng danh tính; đây là 0 hoặc một giá trị tùy thuộc vào việc hoán vị có phải là sự thay đổi nhận dạng theo chu kỳ hay không. 

Tuy nhiên, vì tất cả các hoán vị đều là các hoán vị tùy ý của 1..s, nên hầu hết các hoán vị sẽ không thể giải được riêng lẻ. Cách duy nhất để một hoán vị có thể giải được khi xoay là nếu nó chính xác là một phép quay theo chu kỳ của mảng nhận dạng. Điều đó có nghĩa là nó phải có dạng [k, k+1, ..., s, 1, ..., k-1]. 

Vì vậy, mỗi hoán vị đóng góp một phép quay hợp lệ (một giá trị dịch chuyển) hoặc không đóng góp. 

Bây giờ hãy xem xét một cặp. Giả sử hai hoán vị có giá trị dịch chuyển hợp lệ x và y tương ứng. Nếu chúng ta ghép chúng, chúng ta sẽ chọn một vòng quay r. Sau khi quay r, hoán vị A được giải nếu r bằng x modulo s và B được giải nếu r bằng y modulo s. Vì vậy: 

- Nếu x == y thì cả hai đều được giải quyết. 
- Ngược lại, nhiều nhất có thể giải được một trong cặp đó. 

Điều này làm giảm vấn đề đối với việc ghép nối các chỉ số được gắn nhãn giá trị dịch chuyển hợp lệ hoặc không hợp lệ (-1). Những cái không hợp lệ không bao giờ có thể được giải quyết bất kể ghép nối, vì vậy chúng không đóng góp gì.

Vì vậy, chúng tôi muốn tối đa hóa số lượng cặp trong đó cả hai phần tử có cùng giá trị dịch chuyển. Mỗi cặp như vậy đóng góp 2 mật mã được giải. Mọi thứ khác đóng góp tối đa 1 cho mỗi cặp nếu chúng ta ghép giá trị hợp lệ với không hợp lệ, nhưng không hợp lệ không thể trở thành hợp lệ, vì vậy các cặp đó đóng góp 0 hoặc 1 tùy theo cách giải thích. Vì các hoán vị không hợp lệ không bao giờ có thể giải được nên chúng luôn đóng góp 0, vì vậy chúng ta nên tránh ghép chúng với các hoán vị hợp lệ nếu có thể, nhưng cấu trúc ghép nối buộc phải khớp hoàn toàn. 

Chiến lược tối ưu là nhóm theo giá trị dịch chuyển. Đối với mỗi giá trị dịch chuyển c, nếu có hoán vị cnt[c], chúng ta có thể tạo thành các cặp sàn(cnt[c]/2) mang lại 2 giải được cho mỗi cặp, đóng góp 2 * sàn(cnt[c]/2). Những người hợp lệ chưa ghép đôi đóng góp 0 vì đối tác của họ không thể chia sẻ cùng một ca. 

Điều này trở thành một vấn đề đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi + mô phỏng lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Nhóm theo lớp luân chuyển | O(N · s) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi hoán vị, hãy xác định xem đó có phải là phép quay tuần hoàn của hoán vị nhận dạng hay không. 
2. Nếu đúng như vậy, hãy tính độ lệch xoay của nó r, độ dịch chuyển ánh xạ nó trở lại thứ tự đã sắp xếp. 
3. Đếm xem có bao nhiêu hoán vị tạo ra mỗi r hợp lệ. 
4. Với mỗi r, ghép các hoán vị có cùng r lại với nhau. 
5. Mỗi cặp đóng góp hai mật mã đã được giải, vì vậy hãy thêm 2 * (count[r] // 2). 
6. Tính tổng tất cả r để có kết quả cuối cùng. 

### Tại sao nó hoạt động 

Mỗi hoán vị có nhiều nhất một phép quay để giải quyết nó, bởi vì sự sắp xếp đồng nhất là cứng nhắc dưới sự dịch chuyển theo chu kỳ. Do đó, mọi hoán vị giải được đều thuộc về chính xác một lớp tương đương được xác định bởi độ dịch yêu cầu của nó. Hai hoán vị có thể được giải đồng thời bằng một phép quay chung khi và chỉ khi chúng yêu cầu cùng một sự dịch chuyển. Do đó, việc ghép nối trong một lớp là tối ưu vì việc ghép nối giữa các lớp không thể làm tăng số lượng phần tử có thể giải được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cnt = {}

    for _ in range(n):
        data = list(map(int, input().split()))
        s = data[0]
        p = data[1:]

        # find rotation that makes p sorted [1..s]
        pos1 = p.index(1)
        shift = (s - pos1) % s

        ok = True
        for i in range(s):
            if p[(pos1 + i) % s] != i + 1:
                ok = False
                break

        if ok:
            cnt[shift] = cnt.get(shift, 0) + 1

    ans = 0
    for v in cnt.values():
        ans += (v // 2) * 2

    print(ans)

if __name__ == "__main__":
    solve()
```Trước tiên, mã xác định xem mỗi hoán vị có phải là sự dịch chuyển theo chu kỳ của danh tính hay không bằng cách neo ở vị trí 1 và kiểm tra tính nhất quán tuần tự. Biến`shift`mã hóa phép quay sẽ căn chỉnh hoán vị theo thứ tự đã sắp xếp. 

Chúng tôi chỉ lưu trữ số lượng các lớp ca hợp lệ. Các hoán vị không hợp lệ sẽ bị bỏ qua vì chúng không bao giờ có thể được giải quyết bất kể có ghép nối hay không. 

Cuối cùng, chúng tôi tích lũy các cặp trong mỗi lớp dịch chuyển, thêm hai hoán vị đã giải được cho mỗi cặp. 

Một chi tiết triển khai tinh tế là chúng tôi không cố gắng ghép nối một cách rõ ràng; chúng tôi chỉ đếm tần số. Điều này tránh mọi sự phụ thuộc vào thứ tự và đảm bảo tổng hợp O(1) cho mỗi lớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 1 4 2 3
4 3 4 1 2
4 2 3 4 1
4 2 3 4 1
```Chúng tôi tính toán giá trị xoay vòng: 

| Hoán vị | vị trí(1) | ca | có hiệu lực? | lớp học | 
| --- | --- | --- | --- | --- | 
| 1 4 2 3 | 0 | 0 | không | - | 
| 3 4 1 2 | 2 | 2 | không | - | 
| 2 3 4 1 | 3 | 1 | vâng | 1 | 
| 2 3 4 1 | 3 | 1 | vâng | 1 | 

Chỉ có ca lớp 1 có cỡ 2, tạo thành 1 cặp đóng góp 2 hoán vị được giải. 

Đầu ra:```
2
```Dấu vết này cho thấy chỉ những cấu trúc tuần hoàn giống hệt nhau mới có thể được căn chỉnh hoàn toàn theo một vòng quay chung. 

### Ví dụ 2 

đầu vào:```
4
3 1 2 3
3 2 3 1
3 1 3 2
3 1 2 3
```| Hoán vị | ca | có hiệu lực? | 
| --- | --- | --- | 
| 1 2 3 | 0 | vâng | 
| 2 3 1 | 1 | vâng | 
| 1 3 2 | không hợp lệ | không | 
| 1 2 3 | 0 | vâng | 

Đếm: ca 0 có 2, ca 1 có 1. 

Kết quả là 2 được giải quyết từ
