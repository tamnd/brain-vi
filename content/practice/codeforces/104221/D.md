---
title: "CF 104221D - \u041a\u0430\u0440\u0438\u043c \u0438 \u0434\u043e\u0440\u043e\u0433\u0438"
description: "Chúng ta có một đồ thị đơn giản vô hướng với các giao điểm $n$ và các con đường $m$. Mỗi con đường kết nối hai giao lộ riêng biệt và giữa bất kỳ cặp giao lộ nào có nhiều nhất một đường."
date: "2026-07-01T23:46:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104221
codeforces_index: "D"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104221
solve_time_s: 74
verified: true
draft: false
---

[CF 104221D - \u041a\u0430\u0440\u0438\u043c \u0438 \u0434\u043e\u0440\u043e\u0433\u0438](https://codeforces.com/problemset/problem/104221/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị đơn giản vô hướng với$n$giao lộ và$m$những con đường. Mỗi con đường kết nối hai giao lộ riêng biệt và giữa bất kỳ cặp giao lộ nào có nhiều nhất một đường. Nhiệm vụ là chỉ định hướng cho mọi con đường sao cho sau khi định hướng, không thể bắt đầu ở một giao lộ nào đó và đi theo những con đường được chỉ dẫn mãi trong một vòng. 

Về mặt đồ thị, chúng ta phải chuyển đổi đồ thị vô hướng thành đồ thị có hướng không có chu trình có hướng. Chúng ta có thể tự do chọn hướng của từng cạnh một cách độc lập, nhưng cấu trúc có hướng cuối cùng phải là không theo chu kỳ. Nếu có nhiều hướng hợp lệ, bất kỳ hướng nào trong số đó đều có thể được in. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$các đỉnh và các cạnh. Điều này ngay lập tức loại trừ mọi thứ bậc hai chẳng hạn như thử tất cả các hoán vị của các đỉnh hoặc mô phỏng lặp đi lặp lại sự hình thành chu trình. Chúng tôi cần một$O(n + m)$hoặc$O(m \log n)$sự thi công. 

Một điểm tinh tế là bản thân biểu đồ đầu vào có thể chứa các chu trình và thậm chí nhiều chu trình. Một giả định ngây thơ rằng “trước tiên chúng ta phải phá vỡ chu kỳ” sẽ dẫn đến sự phức tạp không cần thiết. Nhiệm vụ thực sự không phải là xóa các cạnh hoặc sửa đổi cấu trúc mà chỉ đơn thuần là chỉ định hướng. 

Một trường hợp thất bại điển hình xuất phát từ việc cố gắng định hướng tham lam cục bộ mà không có quy tắc toàn cầu. Ví dụ, trong một tam giác$1-2-3-1$, nếu chúng ta định hướng$1 \to 2$,$2 \to 3$, Và$3 \to 1$, chúng ta lập tức tạo ra một chu trình có hướng. Một quy tắc tham lam chỉ dựa trên sự nhất quán cục bộ có thể dễ dàng rơi vào những mâu thuẫn như vậy nếu nó không thực thi một trật tự toàn cầu. 

Một cạm bẫy khác là giả sử trước tiên chúng ta cần phát hiện các chu trình trong đồ thị vô hướng. Điều đó không liên quan, vì ngay cả những đồ thị chứa đầy chu trình vẫn có thể được định hướng thành DAG. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ là cố gắng định hướng từng cạnh một và kiểm tra xem liệu một chu trình được định hướng có xuất hiện sau mỗi nhiệm vụ hay không. Mỗi lần kiểm tra yêu cầu phát hiện chu trình trong biểu đồ có hướng, đó là$O(n + m)$, và chúng tôi làm điều đó$m$lần, dẫn đến$O(m(n + m))$. Với$m$lên đến$2 \cdot 10^5$, tốc độ này quá chậm. 

Hiểu biết sâu sắc về cấu trúc quan trọng là đồ thị có hướng chính xác là không có tính tuần hoàn khi tồn tại một thứ tự tổng thể các đỉnh sao cho mọi cạnh đều hướng về phía trước theo thứ tự đó. Điều này chuyển đổi vấn đề từ “tránh chu kỳ một cách linh hoạt” thành “chọn thứ hạng nhất quán của các đỉnh”. 

Một khi chúng ta chấp nhận rằng chúng ta chỉ cần tổng số đơn đặt hàng thì việc xây dựng sẽ trở nên tầm thường. Chúng tôi chỉ định bất kỳ thứ tự nghiêm ngặt nào cho các đỉnh, ví dụ như nhãn của chúng$1 < 2 < \dots < n$. Khi đó với mọi cạnh vô hướng$(u, v)$, chúng tôi định hướng nó từ nhãn nhỏ hơn đến nhãn lớn hơn. Bất kỳ chu kỳ có hướng nào cũng sẽ bao hàm một chuỗi nhãn tăng dần nghiêm ngặt mà cuối cùng sẽ quay trở lại điểm bắt đầu, điều này là không thể. 

Điều này hoạt động bất kể biểu đồ ban đầu dày đặc hay tuần hoàn đến mức nào, bởi vì hướng này bỏ qua cấu trúc và chỉ dựa vào thứ tự chung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra chu kỳ gia tăng) |$O(m(n+m))$|$O(n+m)$| Quá chậm | 
| Định hướng đặt hàng toàn cầu |$O(n+m)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu đồ tuần hoàn có hướng bằng cách thực thi thứ tự đỉnh nhất quán. 

1. Gán cho mỗi đỉnh một thứ hạng cố định bằng chỉ số của nó từ$1$ĐẾN$n$. Điều này tạo ra một trật tự tổng thể nghiêm ngặt trên tất cả các đỉnh. Lý do giới thiệu thứ tự này là vì bất kỳ định hướng không theo chu kỳ nào cũng phải tương thích với một số thứ tự, vì vậy chúng tôi trực tiếp xây dựng một thứ tự. 
2. Lặp lại tất cả các cạnh khi chúng được đưa vào trong đầu vào. Đối với mỗi cạnh$(u, v)$, so sánh thứ hạng của$u$Và$v$. 
3. Nếu$u < v$, định hướng cạnh như$u \to v$. Nếu không hãy định hướng nó như$v \to u$. Điều này đảm bảo mọi cạnh đều được hướng từ đỉnh có thứ hạng nhỏ hơn đến đỉnh có thứ hạng lớn hơn. 
4. In ra tất cả các cạnh được định hướng theo đúng thứ tự chúng được đọc. 

### Tại sao nó hoạt động 

Giả sử mâu thuẫn rằng một chu trình có hướng tồn tại sau khi định hướng. Theo chu trình này, mọi cạnh phải đi từ chỉ số nhỏ hơn đến chỉ số lớn hơn do quy tắc xây dựng. Điều này ngụ ý rằng khi chúng ta đi qua chu trình, chỉ số đỉnh tăng lên ở mỗi bước. Mức tăng nghiêm ngặt không thể quay trở lại đỉnh bắt đầu, vì điều đó đòi hỏi chỉ số bắt đầu phải đồng thời vừa nhỏ nhất vừa lớn nhất trong chu kỳ. Do đó không có chu trình có hướng nào có thể tồn tại và đồ thị là DAG. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        if u < v:
            edges.append((u, v))
        else:
            edges.append((v, u))

    out = []
    for u, v in edges:
        out.append(f"{u} {v}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp mã hóa ý tưởng đặt hàng. Mỗi cạnh được chuẩn hóa ngay lập tức sao cho hướng của nó tuân theo thứ tự số tự nhiên của các đỉnh. Không cần danh sách kề, DFS hoặc phát hiện chu trình, vì bản thân việc xây dựng đảm bảo tính không tuần hoàn. 

Một lỗi phổ biến là lưu trữ các cạnh và sau đó quyết định hướng không nhất quán. Đặc tính quan trọng là thứ tự toàn cục giống nhau được áp dụng thống nhất cho mọi cạnh, không có ngoại lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
3 2
2 4
4 3
```Chúng tôi xử lý các cạnh theo thứ tự và luôn định hướng từ nhỏ hơn đến lớn hơn. 

| Cạnh | So sánh | Hướng | 
| --- | --- | --- | 
| 1 2 | 1 < 2 | 1 → 2 | 
| 3 2 | 2 < 3 | 2 → 3 | 
| 2 4 | 2 < 4 | 2 → 4 | 
| 4 3 | 3 < 4 | 3 → 4 | 

Đầu ra:```
1 2
2 3
2 4
3 4
```Dấu vết này cho thấy rằng mặc dù biểu đồ ban đầu chứa các chu trình, hướng này sẽ loại bỏ tất cả các cạnh lùi so với trật tự tổng thể. 

### Ví dụ 2 

đầu vào:```
3 3
1 3
2 3
1 2
```| Cạnh | So sánh | Hướng | 
| --- | --- | --- | 
| 1 3 | 1 < 3 | 1 → 3 | 
| 2 3 | 2 < 3 | 2 → 3 | 
| 1 2 | 1 < 2 | 1 → 2 | 

Đầu ra:```
1 3
2 3
1 2
```Ví dụ này nhấn mạnh rằng đồ thị có hướng thu được là không có chu kỳ mặc dù đồ thị ban đầu là một hình tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được xử lý một lần với định dạng đầu ra và so sánh theo thời gian không đổi | 
| Không gian |$O(m)$| Chúng tôi lưu trữ danh sách các cạnh trước khi in | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì cả hai$n$Và$m$nhiều nhất là$2 \cdot 10^5$và thuật toán chỉ thực hiện công tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        if u < v:
            edges.append((u, v))
        else:
            edges.append((v, u))
    return "\n".join(f"{u} {v}" for u, v in edges)

# provided sample
assert run("""4 4
1 2
3 2
2 4
4 3
""") == """1 2
2 3
2 4
3 4"""

# chain
assert run("""3 2
1 2
2 3
""") == """1 2
2 3"""

# reversed input order
assert run("""3 3
3 2
2 1
3 1
""") == """2 3
1 2
1 3"""

# complete triangle
assert run("""3 3
1 2
2 3
3 1
""") == """1 2
2 3
1 3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | định hướng tuyến tính | tính đúng đắn cơ bản | 
| cạnh đảo ngược | đặt hàng nhất quán | xử lý đối xứng | 
| tam giác | kết quả theo chu kỳ | loại bỏ chu kỳ | 

## Vỏ cạnh 

Một biểu đồ tuần hoàn dày đặc chẳng hạn như biểu đồ hoàn chỉnh được xử lý chính xác vì mọi cạnh vẫn được định hướng theo cùng một thứ tự số tổng thể. Ví dụ, trong một bộ ba được kết nối đầy đủ, mọi cạnh đều trở nên nhất quán với$1 < 2 < 3$, tạo ra một hệ thống phân cấp chặt chẽ chứ không phải là một chu trình. 

Đồ thị một cạnh hoạt động bình thường vì phép so sánh ngay lập tức chỉ định hướng mà không có bất kỳ sự phụ thuộc nào vào các cạnh khác. Ngay cả trong trường hợp tối thiểu này, bất biến mà tất cả các cạnh tuân theo cùng một thứ tự vẫn có giá trị. 

Đồ thị trong đó các cạnh đầu vào được đưa ra theo thứ tự tùy ý không ảnh hưởng đến độ chính xác vì hướng không phụ thuộc vào chuỗi đầu vào. Mỗi cạnh được xử lý riêng biệt nhưng luôn tuân theo cùng một quy tắc chung.
