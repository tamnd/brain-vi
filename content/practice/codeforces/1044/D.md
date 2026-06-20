---
title: "CF 1044D - Truy vấn khấu trừ"
description: "Chúng tôi đang xử lý một mảng khái niệm cực kỳ lớn được lập chỉ mục từ 0 đến $2^{30}-1$, nhưng chúng tôi thực sự chưa bao giờ lưu trữ các giá trị của nó."
date: "2026-06-16T17:34:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1044
codeforces_index: "D"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Final Round"
rating: 2400
weight: 1044
solve_time_s: 420
verified: false
draft: false
---

[CF 1044D - Truy vấn khấu trừ](https://codeforces.com/problemset/problem/1044/D) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dsu 
**Thời gian giải:** 7 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một mảng khái niệm cực lớn được lập chỉ mục từ 0 đến$2^{30}-1$, nhưng chúng tôi không bao giờ thực sự lưu trữ giá trị của nó. Thay vào đó, chúng tôi nhận được thông tin về mối quan hệ XOR giữa các phân đoạn của mảng này và phải trả lời các truy vấn về việc liệu XOR phân đoạn đã được xác định hay vẫn chưa rõ ràng. 

Mỗi bản cập nhật đưa ra một ràng buộc có dạng “XOR của các giá trị trên một phạm vi$[l, r]$bằng$x$”. Mỗi truy vấn yêu cầu XOR của một phạm vi$[l, r]$, nhưng chỉ khi giá trị đó được xác định duy nhất bởi tất cả các ràng buộc được chấp nhận trước đó. Nếu không chúng ta phải trả về -1. 

Điều tinh tế quan trọng là các bản cập nhật có thể mâu thuẫn với những bản cập nhật trước đó. Khi xảy ra mâu thuẫn, bản cập nhật đó sẽ bị bỏ qua hoàn toàn, nghĩa là nó không thay đổi hệ thống ràng buộc đã biết. Điều này làm cho cấu trúc trở nên linh hoạt: chúng tôi đang duy trì một hệ thống các phương trình XOR tuyến tính đang phát triển trên cấu trúc tiền tố, nhưng chỉ giữ lại những phương trình nhất quán. 

Đầu vào được mã hóa làm phức tạp việc đọc: mọi truy vấn đều bị che dấu XOR bởi câu trả lời trước đó, do đó phạm vi thực tế phụ thuộc vào kết quả đầu ra đang phát triển. Điều này buộc phải có một giải pháp trực tuyến hoàn toàn. 

Kích thước hạn chế, lên đến$2 \cdot 10^5$truy vấn, loại trừ bất cứ điều gì chạm trực tiếp vào phạm vi hoặc cố gắng duy trì một mảng rõ ràng hoặc mảng XOR tiền tố có kích thước$2^{30}$. Ngay cả việc nén tọa độ trên tất cả các chỉ mục cũng phải được xử lý một cách lười biếng vì các chỉ mục được tiết lộ trực tuyến. 

Tất cả các trường hợp quan trọng đều gắn liền với sự không nhất quán và kiến ​​thức một phần. 

Trường hợp tinh tế đầu tiên là khi các ràng buộc tạo thành một chu kỳ tạo ra mâu thuẫn. Ví dụ, giả sử chúng ta đã biết$a_1 \oplus a_2 = 5$Và$a_2 \oplus a_3 = 7$, và sau đó chúng tôi nhận được$a_1 \oplus a_3 = 1$. Nếu ràng buộc thứ ba này không khớp với XOR ngụ ý thì nó phải được bỏ qua. Một cách tiếp cận ngây thơ luôn chèn các ràng buộc sẽ làm sập hệ thống một cách không chính xác và tạo ra các câu trả lời sai. 

Trường hợp thứ hai là các truy vấn sớm khi không tồn tại ràng buộc nào. Bất kỳ truy vấn XOR phạm vi nào cũng phải trả về -1, vì có thể thực hiện nhiều phép gán cho mảng. 

Trường hợp thứ ba là các phân đoạn được xác định một phần: ngay cả khi tồn tại một số ràng buộc, phân đoạn được truy vấn vẫn có thể phụ thuộc vào một vùng không bị ràng buộc, do đó XOR của nó không cố định duy nhất. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mỗi ràng buộc như một phương trình trên$a_i$giá trị và cố gắng duy trì tất cả các phương trình, tính toán lại tính nhất quán sau mỗi lần cập nhật. Người ta có thể cố gắng duy trì tất cả các phương trình tuyến tính đã biết và giải chúng bằng cách sử dụng phép loại bỏ Gaussian trên XOR. Điều này hoạt động về mặt khái niệm vì XOR là phần bổ sung trên GF(2), nhưng mỗi bản cập nhật đều ảnh hưởng đến khả năng$O(n)$các biến và mỗi truy vấn yêu cầu giải quyết một hệ thống có thể phát triển tới$O(q)$hạn chế. Trong trường hợp xấu nhất, việc loại bỏ Gaussian sẽ yêu cầu$O(q^3)$hoạt động vượt quá giới hạn cho phép$2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không cần các giá trị riêng lẻ của$a_i$. Chúng tôi chỉ cần sự khác biệt XOR giữa các chỉ số. Nếu chúng tôi sửa lỗi diễn giải gốc bằng cách sử dụng XOR tiền tố thì mỗi ràng buộc sẽ trở thành mối quan hệ giữa các giá trị tiền tố. Cụ thể, nếu chúng ta định nghĩa$p[i] = a_0 \oplus a_1 \oplus \cdots \oplus a_{i-1}$, thì bất kỳ phạm vi XOR nào cũng sẽ trở thành$p[r+1] \oplus p[l]$. Các ràng buộc sau đó trở thành sự bình đẳng giữa hai nút tiền tố. 

Điều này biến vấn đề thành việc duy trì kết nối trong biểu đồ trong đó các cạnh mang trọng số XOR. Mỗi chỉ mục là một nút và một ràng buộc kết nối$l$Và$r+1$với sự khác biệt XOR đã biết. Nếu chúng tôi kết nối các thành phần với khoảng cách XOR nhất quán, chúng tôi có thể trả lời các truy vấn nếu cả hai điểm cuối đều thuộc về cùng một thành phần; mặt khác giá trị không xác định. 

Để hỗ trợ những mâu thuẫn, chúng tôi cần một DSU không chỉ lưu trữ kết nối mà còn lưu trữ khoảng cách XOR tới cấp độ gốc. Khi hợp nhất hai thành phần, chúng tôi tính toán trọng số cần thiết; nếu nó xung đột với thông tin hiện có, chúng tôi sẽ loại bỏ ràng buộc đó. 

Do đó, vấn đề giảm xuống còn DSU có trọng số với thế XOR. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (phương trình tuyến tính) |$O(q^3)$|$O(q)$| Quá chậm | 
| DSU có trọng số với XOR |$O(q \alpha(q))$|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích từng vị trí$i$như một nút đại diện cho tiền tố XOR$p[i]$. Chúng tôi duy trì một DSU trong đó mỗi nút lưu trữ nút cha và một giá trị`xr[i]`nghĩa là XOR từ nút đến nút cha của nó. 

Mỗi bản cập nhật đưa ra một ràng buộc giữa hai nút và chúng tôi hợp nhất các thành phần hoặc phát hiện sự không nhất quán. 

1. Khởi tạo DSU trên tất cả các chỉ số có thể xuất hiện. Vì các chỉ số đều lớn nhưng chỉ tối đa$2q$các điểm cuối riêng biệt có thể xuất hiện, chúng tôi phân bổ DSU một cách lười biếng bằng cách sử dụng từ điển hoặc cấu trúc được phân bổ trước cho các chỉ mục nén. Chúng tôi coi mỗi điểm cuối là một nút. 
2. Đối với mỗi ràng buộc$l, r, x$, chuyển đổi nó thành mối quan hệ giữa các nút tiền tố:$p[l] \oplus p[r+1] = x$. Điều này đòi hỏi phải điều trị$r+1$cũng như một nút. 
3. Khi xử lý một ràng buộc, hãy tìm nghiệm DSU của$l$Và$r+1$, cùng với khoảng cách XOR từ mỗi nút đến gốc của nó. 
4. Nếu cả hai nút đã thuộc về cùng một thành phần, hãy xác minh tính nhất quán bằng cách kiểm tra xem XOR ngụ ý có bằng không$x$. Nếu không bằng thì bỏ qua ràng buộc. 
5. Nếu chúng thuộc về các thành phần khác nhau, hãy hợp nhất chúng bằng cách gắn một gốc này vào gốc kia và đặt độ lệch XOR sao cho phương trình giữ nguyên. Điều này bảo tồn tất cả các mối quan hệ đã biết trước đó. 
6. Đối với các truy vấn$[l, r]$, tính toán chênh lệch XOR tiền tố giữa nút$l$và nút$r+1$. Nếu chúng không được kết nối, xuất -1 vì giá trị phụ thuộc vào thông tin chưa biết. 
7. Áp dụng mã hóa: trước mỗi truy vấn hoặc cập nhật, đầu vào XOR có`last`, sau đó chuẩn hóa phạm vi. Cập nhật`last`chỉ sau loại truy vấn 2. 

DSU duy trì một bất biến: trong mỗi thành phần, mọi nút đều có một giá trị XOR nhất quán liên quan đến gốc thành phần. Do đó, bất kỳ hai nút nào trong cùng một thành phần đều có sự khác biệt XOR được xác định duy nhất. 

## Tại sao nó hoạt động 

Mỗi ràng buộc đưa ra một phương trình tuyến tính trên GF(2). DSU nén từng thành phần được kết nối vào một hệ thống trong đó tất cả các phương trình đã được thỏa mãn. Các khác biệt XOR được lưu trữ đóng vai trò là các tiềm năng xác định việc gán nhất quán các giá trị tiền tố cho đến độ lệch toàn cục tùy ý cho mỗi thành phần. Nếu một ràng buộc mới mâu thuẫn với các tiềm năng hiện có bên trong một thành phần thì không có phép gán nào tồn tại có thể đáp ứng cả hai, do đó ràng buộc đó bị từ chối. Điều này đảm bảo rằng cấu trúc được duy trì luôn thể hiện một giải pháp từng phần hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.xor = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            orig = self.parent[x]
            self.parent[x] = self.find(self.parent[x])
            self.xor[x] ^= self.xor[orig]
        return self.parent[x]

    def get_xor(self, x):
        self.find(x)
        return self.xor[x]

    def union(self, a, b, w):
        ra = self.find(a)
        rb = self.find(b)
        xa = self.get_xor(a)
        xb = self.get_xor(b)

        if ra == rb:
            return (xa ^ xb) == w

        if self.rank[ra]()
```
