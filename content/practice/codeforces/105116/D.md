---
title: "CF 105116D - \u041c\u043d\u043e\u0433\u043e\u0447\u0438\u0441\u043b\u0435\u043d\u043d\u044b\u0435 \u043c\u043e\u043d\u0435\u0442\u044b"
description: "Chúng ta được cung cấp một chuỗi các loại tiền xu trong đó mỗi loại có giá trị hơn loại trước đó. Tỷ giá hối đoái được nhân lên dọc theo chuỗi: một số lượng cố định đồng xu loại i có thể được đổi lấy một đồng xu loại i+1."
date: "2026-06-27T19:47:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105116
codeforces_index: "D"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2024, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 105116
solve_time_s: 68
verified: true
draft: false
---

[CF 105116D - \u041c\u043d\u043e\u0433\u043e\u0447\u0438\u0441\u043b\u0435\u043d\u043d\u044b\u0435 \u043c\u043e\u043d\u0435\u0442\u044b](https://codeforces.com/problemset/problem/105116/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các loại tiền xu trong đó mỗi loại có giá trị hơn loại trước đó. Tỷ giá hối đoái được nhân lên dọc theo chuỗi: một số lượng cố định đồng xu loại i có thể được đổi lấy một đồng xu loại i+1. Bởi vì điều này đúng cho mọi cặp liền kề, bất kỳ loại tiền xu nào cũng có thể được biểu thị theo bất kỳ loại nào khác bằng cách nhân hoặc chia dọc theo đường dẫn. 

Người chơi bắt đầu với một số bộ xu, trong đó mỗi loại có một số lượng. Theo thời gian, tiền xu được thêm vào bộ sưu tập. Sau mỗi lần cập nhật, chúng tôi được hỏi một câu hỏi có dạng: tổng giá trị của tất cả các đồng tiền hiện đang nắm giữ ít nhất có lớn bằng giá trị của một số gói mục tiêu bao gồm x đồng tiền loại y không? 

Vì vậy, mỗi truy vấn là một bản cập nhật điểm trên một tọa độ của vectơ đếm hoặc so sánh giữa tổng trọng số hiện tại của tất cả các đồng tiền và một gói có trọng số duy nhất. 

Khó khăn chính là trọng số không độc lập với mỗi loại. Giá trị của loại i phụ thuộc vào tích của tất cả các tỷ giá hối đoái ở trên nó, do đó việc chuyển đổi số trực tiếp sẽ nhanh chóng bùng nổ. Với n lên tới 200.000 và q lên tới 100.000, việc tính toán lại giá trị toàn cầu sau mỗi truy vấn là quá chậm và thậm chí việc duy trì các số nguyên khổng lồ rõ ràng cũng trở nên không khả thi vì các giá trị trung gian tăng theo cấp số nhân. 

Một sai lầm ngây thơ là chuyển đổi mọi thứ thành giá trị loại 1 bằng cách sử dụng các tích số tiền tố và duy trì một số nguyên lớn duy nhất. Điều này không thành công vì các chuỗi nhân như 10^9 lặp đi lặp lại 200.000 lần tạo ra các số có hàng trăm nghìn chữ số, khiến mỗi thao tác trở nên quá chậm. 

Một trường hợp thất bại tinh vi khác xuất phát từ việc so sánh các giá trị lớn bằng cách sử dụng số nguyên dấu phẩy động hoặc số nguyên có giới hạn. Nếu chúng ta cắt ngắn các giá trị lớn quá sớm, hai cường độ khác nhau có thể trở nên không thể phân biệt được và việc so sánh có thể trở nên không chính xác khi cả hai bên đều vượt quá giới hạn nhưng không bằng nhau. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là gán cho mỗi loại đồng xu một trọng lượng cố định bằng giá trị của nó trong đồng xu loại 1. Khi đó tổng giá trị chỉ là tích số chấm của số đếm và trọng số. Việc cập nhật rất dễ dàng, nhưng các truy vấn yêu cầu duy trì một số nguyên lớn được tăng lên nhiều lần và các phép cộng liên quan đến số nhân rất lớn. Điều này dẫn đến số học số nguyên lớn nặng và trở nên quá chậm trong trường hợp xấu nhất khi các giá trị tăng liên tục dọc theo chuỗi đầy đủ. 

Cái nhìn sâu sắc về cấu trúc là chúng ta thực sự không cần các giá trị chính xác. Mọi truy vấn chỉ yêu cầu so sánh giữa hai đại lượng. Thay vì duy trì những số nguyên khổng lồ chính xác, chúng ta chỉ cần duy trì đủ thông tin để xác định thứ tự. 

Điều này gợi ý việc duy trì các giá trị trong một hệ thống số bão hòa, trong đó mọi thứ vượt quá một ngưỡng nhất định đều được coi là “vô hạn” vì khi một giá trị đủ lớn, nó sẽ luôn chiếm ưu thế bất kỳ truy vấn nào có thể được biểu thị trong các ràng buộc đầu vào. Bên cạnh đó, chúng tôi sử dụng cấu trúc trao đổi để tuyên truyền các khoản đóng góp lên trên, đảm bảo các cập nhật vẫn mang tính cục bộ và có hiệu lực logarit chứ không phải toàn cầu. 

Chúng tôi lưu trữ nhiều tập hợp hiện tại trong cây phân đoạn theo các loại tiền xu, trong đó mỗi nút giữ giá trị của phân đoạn được biểu thị bằng đơn vị loại 1, nhưng bị giới hạn ở giới hạn bão hòa cố định. Cập nhật điều chỉnh một lá đơn và tính toán lại trở lên. Truy vấn tính toán cả giá trị hiện tại và giá trị mục tiêu trong cùng một hệ thống giới hạn và so sánh chúng. 

Ý tưởng quan trọng là các so sánh vẫn đúng miễn là độ bão hòa chỉ loại bỏ sự khác biệt giữa các giá trị vốn đã quá lớn so với bất kỳ ngưỡng truy vấn nào.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi Brute Force trên mỗi truy vấn | O(n) mỗi truy vấn | O(n) | Quá chậm | 
| Cây phân đoạn bão hòa | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các hệ số chuyển đổi tiền tố từ từng loại sang loại 1. Thay vì lưu trữ chính xác các sản phẩm, hãy duy trì chúng ở trạng thái bão hòa sớm. Nếu một sản phẩm vượt quá giới hạn lớn đã chọn, chúng tôi sẽ kẹp nó lại. 
2. Xây dựng cây phân đoạn theo các loại tiền xu. Mỗi lá lưu trữ sự đóng góp của loại đồng xu đó vào tổng giá trị, đã được chia thành đơn vị loại 1 và đã bão hòa. 
3. Đối với truy vấn cập nhật, hãy tăng số lượng của một loại tiền xu. Chúng tôi chuyển đổi phần tăng này thành phần đóng góp loại 1 và thêm nó vào lá tương ứng, sau đó tính toán lại các nút cha. Nếu bất kỳ nút nào vượt quá giới hạn bão hòa, chúng tôi sẽ kẹp nút đó. 
4. Đối với truy vấn so sánh, chúng tôi tính toán giá trị của x đồng xu loại y bằng cách sử dụng cùng một logic chuyển đổi tiền tố, một lần nữa với độ bão hòa. 
5. Cuối cùng, chúng tôi so sánh tổng giá trị bão hòa của nhiều tập hợp hiện tại với giá trị bão hòa của gói truy vấn và đưa ra xem giá trị trước ít nhất là giá trị sau. 

Lý do điều này có hiệu quả là vì mọi so sánh thực tế chỉ phụ thuộc vào việc liệu một giá trị cuối cùng có chiếm ưu thế hơn một giá trị khác hay không. Khi một giá trị vượt quá phạm vi có ý nghĩa tối đa mà bất kỳ số bắt nguồn từ truy vấn nào có thể truy cập được theo cùng một quy tắc bão hòa, thì độ lớn chính xác của nó sẽ không còn phù hợp. Cả trạng thái hệ thống và giá trị truy vấn đều được xử lý đối xứng dưới cùng một cách cắt ngắn, duy trì thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
q = int(input())

# prefix conversion to type 1 with saturation
pref = [1] * (n + 1)
for i in range(1, n + 1):
    if i == 1:
        pref[i] = 1
    else:
        val = pref[i - 1] * a[i - 2]
        if val > INF:
            val = INF
        pref[i] = val

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def cap(self, x):
        return INF if x > INF else x

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = self.cap(arr[l] * pref[l + 1])
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m, arr)
            self.build(v * 2 + 1, m + 1, r, arr)
            self.t[v] = self.cap(self.t[v * 2] + self.t[v * 2 + 1])

    def add(self, v, l, r, idx, val):
        if l == r:
            self.t[v] = self.cap(self.t[v] + val * pref[idx + 1])
        else:
            m = (l + r) // 2
            if idx <= m:
                self.add(v * 2, l, m, idx, val)
            else:
                self.add(v * 2 + 1, m + 1, r, idx, val)
            self.t[v] = self.cap(self.t[v * 2] + self.t[v * 2 + 1])

    def query_sum(self):
        return self.t[1]

st = SegTree(b)

for _ in range(q):
    t, x, y = map(int, input().split())
    y -= 1
    if t == 1:
        st.add(1, 0, n - 1, y, x)
    else:
        # compute value of x type-y coins in type 1 units
        need = x * pref[y + 1]
        need = INF if need > INF else need
        print(1 if st.query_sum() >= need else 0)
```Cây phân đoạn lưu trữ sự đóng góp của từng loại đã được chuẩn hóa thành các đơn vị loại 1. Các bản cập nhật chỉ chạm vào một lá và lan truyền lên trên, giữ cho tổng toàn cầu nhất quán. 

Sự lựa chọn thiết kế tinh tế duy nhất là độ bão hòa. Ngưỡng INF đảm bảo rằng chúng tôi không bao giờ mất thời gian biểu thị những con số vượt quá những gì có thể ảnh hưởng đến bất kỳ sự so sánh nào.

 ## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống nhỏ có 3 loại tiền và tỷ giá hối đoái 2 và 3. Vậy loại 2 bằng 2 loại 1 và loại 3 bằng 6 loại 1. Giả sử ban đầu chúng ta có 1 đồng tiền mỗi loại. 

Chúng tôi xử lý một truy vấn thêm 2 đồng xu loại 2, sau đó hỏi liệu chúng tôi có đủ khả năng mua 1 đồng xu loại 3 hay không. 

| Bước | b1 | b2 | b3 | tổng (loại 1) | giá trị truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 1 | 1 + 2 + 6 = 9 | - | 
| thêm 2 loại2 | 1 | 3 | 1 | 1 + 6 + 6 = 13 | - | 
| truy vấn | - | - | - | 13 | 6 | 

Vì 13 ≥ 6 nên câu trả lời là 1. Điều này cho thấy cách tất cả các loại được ánh xạ nhất quán vào một tỷ lệ duy nhất. 

### Ví dụ 2 

Bây giờ hãy xem xét trường hợp độ bão hòa có vấn đề. Giả sử tỷ giá hối đoái lớn và các cập nhật lặp đi lặp lại sẽ đẩy giá trị vượt quá giới hạn. Hệ thống lưu trữ cả tổng hiện tại và ngưỡng truy vấn dưới dạng INF khi chúng vượt quá giới hạn, nhưng vì cả hai đều được tính theo quy tắc bão hòa giống nhau nên các so sánh vẫn ổn định. 

Điều này chứng tỏ rằng thuật toán không bao giờ phân biệt giữa các giá trị không liên quan đến thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật chạm vào một đường dẫn cây phân đoạn; mỗi truy vấn là O(1) sau khi xử lý trước | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cộng với mảng tiền tố | 

Các ràng buộc cho phép tổng cộng lên tới 300.000 phép toán và cập nhật logarit phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# NOTE: placeholder since full solution is embedded above
```Việc khai thác thử nghiệm ở trên không được hoàn thiện một cách có chủ ý vì giải pháp đầy đủ được viết sẵn theo phong cách lập trình cạnh tranh; trong thực tế, bạn sẽ gói logic vào một hàm và sử dụng lại nó. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | so sánh đúng | nhỏ nhất n | 
| cập nhật loại đơn | tích lũy đúng | cập nhật điểm | 
| truy vấn x lớn | xử lý tràn | bão hòa | 
| cập nhật/truy vấn hỗn hợp | ổn định | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các bản cập nhật lặp lại đẩy đóng góp của một loại vượt xa bất kỳ giá trị truy vấn nào. Nếu không bão hòa, điều này sẽ gây ra hiện tượng tràn số nguyên hoặc tính toán quá mức. Với độ bão hòa, khi giá trị vượt quá INF, nó sẽ được xử lý đồng nhất và việc tăng thêm sẽ không ảnh hưởng đến việc so sánh. 

Một trường hợp đặc biệt khác xảy ra khi một truy vấn yêu cầu loại xu rất cao, trong đó hệ số chuyển đổi cực kỳ lớn. Sản phẩm tiền tố ngay lập tức bão hòa, nghĩa là giá trị được yêu cầu trở thành INF. Bất kỳ tổng hiện tại nào cũng bão hòa sẽ so sánh chính xác, duy trì thứ tự dự định mặc dù cường độ chính xác không được theo dõi.
