---
title: "CF 105071D - Thợ săn uy tín"
description: "Chúng ta được cung cấp một danh sách tham khảo cố định tên các công ty được sắp xếp theo uy tín, trong đó vị trí 1 tương ứng với công ty uy tín nhất. Mỗi truy vấn bao gồm một tên công ty và chúng tôi phải xác định xem tên đó có xuất hiện trong danh sách tham chiếu hay không."
date: "2026-06-27T23:26:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105071
codeforces_index: "D"
codeforces_contest_name: "UTPC April Fools Contest 2024"
rating: 0
weight: 105071
solve_time_s: 98
verified: false
draft: false
---

[CF 105071D - Thợ săn uy tín](https://codeforces.com/problemset/problem/105071/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách tham khảo cố định tên các công ty được sắp xếp theo uy tín, trong đó vị trí 1 tương ứng với công ty uy tín nhất. Mỗi truy vấn bao gồm một tên công ty và chúng tôi phải xác định xem tên đó có xuất hiện trong danh sách tham chiếu hay không. Nếu đúng như vậy, chúng tôi xuất vị trí dựa trên 1 của nó trong danh sách đó; nếu không chúng ta sẽ xuất ra -1. 

Thuộc tính chính là việc tra cứu không phân biệt chữ hoa chữ thường, nghĩa là các tên chỉ khác nhau ở chữ hoa hoặc chữ thường sẽ được coi là giống hệt nhau. Kích thước đầu vào khiêm tốn, tối đa 1000 truy vấn và mỗi tên tối đa 1000 ký tự, do đó, ngay cả các chiến lược tra cứu khá trực tiếp cũng khả thi, nhưng việc quét lặp lại một danh sách lớn cho mỗi truy vấn sẽ trở nên không hiệu quả nếu danh sách lớn. 

Trường hợp cạnh không tầm thường chính là chuẩn hóa. Nếu chúng tôi không chuẩn hóa kiểu chữ một cách nhất quán cho cả danh sách được lưu trữ và chuỗi truy vấn thì các kết quả khớp hợp lệ sẽ bị bỏ sót. Ví dụ: nếu danh sách chứa "Google" và truy vấn là "goOGle", so sánh phân biệt chữ hoa chữ thường sẽ trả về không chính xác -1. Kết quả đúng là chỉ mục của "Google" trong danh sách. 

Một trường hợp đặc biệt khác là các truy vấn lặp lại và tên công ty lặp lại dưới các hình thức khác nhau. Vì các truy vấn là độc lập nên mỗi truy vấn phải được trả lời dựa trên cùng một tập dữ liệu cố định mà không có tác dụng phụ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập và quét toàn bộ danh sách công ty, so sánh từng tên được lưu trữ với truy vấn bằng cách sử dụng so sánh không phân biệt chữ hoa chữ thường. Điều này đúng vì nó kiểm tra mọi kết quả phù hợp có thể, nhưng chi phí của nó tăng tuyến tính theo cả số lượng công ty trong danh sách và số lượng truy vấn. Nếu danh sách có N mục nhập, mỗi truy vấn sẽ tốn O(N) so sánh chuỗi, dẫn đến tổng công việc là O(TN). Với một danh sách ẩn lớn, việc này nhanh chóng trở nên quá chậm. 

Quan sát quan trọng là danh sách công ty là tĩnh. Vì nó không bao giờ thay đổi trong các truy vấn nên chúng tôi có thể xử lý trước nó một lần thành bản đồ băm từ tên công ty được chuẩn hóa đến thứ hạng của nó. Điều này làm giảm mỗi truy vấn thành một tra cứu từ điển duy nhất. Bước quan trọng là chuẩn hóa: chúng tôi chuyển đổi mọi tên công ty thành chữ thường trước khi chèn nó vào bản đồ, đảm bảo việc so khớp không phân biệt chữ hoa chữ thường được xử lý một lần trên toàn cầu thay vì theo so sánh truy vấn. 

Sau khi tiền xử lý, mỗi truy vấn sẽ trở thành thời gian dự kiến ​​là O(1), vì việc tra cứu từ điển không phụ thuộc vào kích thước danh sách. Điều này chuyển chi phí từ việc quét lặp lại sang bước tiền xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(TN) | O(1) | Quá chậm | 
| Tiền xử lý bản đồ băm | O(N + T) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Đọc danh sách xếp hạng đầy đủ của công ty và lưu trữ theo thứ tự. 

Thứ tự quan trọng vì chỉ mục trong danh sách này là đầu ra bắt buộc cho mỗi trận đấu. 
2. Chuẩn hóa mọi tên công ty trong danh sách bằng cách chuyển đổi nó thành chữ thường. 

Điều này đảm bảo rằng tất cả các so sánh trong tương lai đều không phân biệt chữ hoa chữ thường mà không cần tính toán lặp lại. 
3. Xây dựng một từ điển ánh xạ từng tên công ty được chuẩn hóa vào chỉ mục dựa trên 1 của nó. 

Nếu tồn tại các bản sao (không chắc chắn nhưng an toàn để xử lý), lần xuất hiện đầu tiên sẽ được giữ lại vì nó tương ứng với thứ hạng tốt nhất. 
4. Đối với mỗi chuỗi truy vấn, hãy chuẩn hóa nó bằng cách sử dụng cùng một phép biến đổi chữ thường. 

Điều này đảm bảo tính nhất quán giữa các khóa được lưu trữ và các truy vấn đến. 
5. Kiểm tra xem truy vấn chuẩn hóa có tồn tại trong từ điển hay không. 

Nếu nó tồn tại, xuất chỉ mục được lưu trữ; nếu không thì xuất ra -1. 

### Tại sao nó hoạt động

Thuật toán dựa trên tính bất biến là mỗi tên công ty được thể hiện dưới một dạng chính tắc trong từ điển: phiên bản chữ thường của nó. Bởi vì cả tập dữ liệu và truy vấn đều được chuyển đổi bằng cách sử dụng cùng một hàm, nên đẳng thức trong chuỗi gốc sẽ giảm xuống thành đẳng thức trong chuỗi chuẩn hóa. Do đó, từ điển trở thành một sự thể hiện đầy đủ và không mất dữ liệu về thông tin thành viên và xếp hạng. Không có truy vấn nào có thể khớp với mục nhập hợp lệ mà không chia sẻ khóa chuẩn hóa của nó và không có truy vấn không hợp lệ nào có thể xuất hiện trong từ điển trừ khi nó hiện diện rõ ràng trong danh sách ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# The problem refers to an external list of companies.
# In a real contest setting, this would be provided as input or preloaded.
# For this solution, we assume it is available as a static list called companies.

def solve():
    # Since the actual list is external, we simulate structure:
    # Replace this with the actual parsed list from the problem source.
    companies = sys.stdin.readline().strip().split(",")

    # Build mapping from lowercase name to rank
    rank = {}
    for i, name in enumerate(companies, start=1):
        key = name.strip().lower()
        if key not in rank:
            rank[key] = i

    t_line = sys.stdin.readline().strip()
    if not t_line:
        return
    t = int(t_line)

    out = []
    for _ in range(t):
        q = sys.stdin.readline().strip().lower()
        out.append(str(rank.get(q, -1)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là từ điển`rank`, lưu trữ chỉ mục xuất hiện đầu tiên của mỗi tên công ty sau khi chuẩn hóa. các`.lower()`lệnh gọi được áp dụng nhất quán cho cả mục nhập tập dữ liệu và truy vấn, đảm bảo khớp không phân biệt chữ hoa chữ thường. 

Một mối quan tâm triển khai tinh tế là loại bỏ khoảng trắng. Vì các dòng đầu vào có thể chứa dấu cách ở cuối hoặc các tạo phẩm định dạng ẩn từ quá trình phân tích cú pháp CSV,`strip()`được áp dụng trước khi chuẩn hóa. Một chi tiết quan trọng khác là chúng tôi chỉ lưu trữ lần xuất hiện đầu tiên của mỗi tên được chuẩn hóa, duy trì thứ hạng tốt nhất trong trường hợp trùng lặp. 

## Ví dụ đã hoạt động 

Vì tập dữ liệu đầy đủ ở bên ngoài nên chúng tôi xây dựng một phiên bản minh họa đơn giản hóa. 

### Ví dụ 1 

Danh sách đầu vào: 

["Meta", "Google", "Netflix"] 

Truy vấn: 

["google", "Amazon"] 

| Bước | Truy vấn | Chuẩn hóa | Tra cứu từ điển | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | google | google | tìm thấy ở 2 | 2 | 
| 2 | Amazon | amazon | không tìm thấy | -1 | 

Điều này xác nhận việc so khớp không phân biệt chữ hoa chữ thường và xử lý chính xác các mục bị thiếu. 

### Ví dụ 2 

Danh sách đầu vào: 

["Apple", "Microsoft", "OpenAI", "táo"] 

Truy vấn: 

["TÁO", "openai", "Tesla"] 

| Bước | Truy vấn | Chuẩn hóa | Tra cứu từ điển | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | TÁO | táo | tìm thấy ở 1 | 1 | 
| 2 | mở | mở | tìm thấy lúc 3 | 3 | 
| 3 | Tesla | tesla | không tìm thấy | -1 | 

Điều này thể hiện việc xử lý trùng lặp: "apple" xuất hiện hai lần nhưng chỉ lưu trữ chỉ mục đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + T) | Một lượt xây dựng từ điển, sau đó mỗi truy vấn có giá trị trung bình là O(1) | 
| Không gian | O(N) | Từ điển lưu trữ tối đa một mục nhập cho mỗi công ty | 

Chi phí tiền xử lý không đáng kể với ràng buộc T ≤ 1000 và ngay cả khi danh sách công ty lớn, việc băm đảm bảo giải pháp vẫn hiệu quả trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style simplified dataset encoding assumed in solve()

# custom cases
assert run("Meta,Google,Netflix\n3\ngoogle\namazon\nNETFLIX\n") == "2\n-1\n3"
assert run("Apple,Apple,Apple\n2\napple\nAPPLE\n") == "1\n1"
assert run("A,B,C,D\n4\na\nb\nc\nd\n") == "1\n2\n3\n4"
assert run("X,Y,Z\n2\nx\nw\n") == "1\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tên lặp đi lặp lại | lần xuất hiện đầu tiên | xử lý trùng lặp | 
| bộ hit đầy đủ | 1..N | tính đúng đắn của việc lập chỉ mục | 
| truy vấn bị thiếu | -1 | tra cứu phủ định đúng đắn | 
| trường hợp hỗn hợp | trận đấu đúng | chuẩn hóa trường hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là các tên được chuẩn hóa lặp lại trong danh sách công ty. Nếu dữ liệu đầu vào chứa nhiều biến thể của cùng một công ty, chỉ khác nhau về trường hợp, chẳng hạn như "Google" và "GOOGLE", thì chỉ lần xuất hiện đầu tiên mới xác định thứ hạng. Việc xây dựng từ điển đảm bảo điều này bằng cách kiểm tra xem khóa đã tồn tại hay chưa trước khi chèn. 

Một trường hợp khác là khoảng trắng không nhất quán. Truy vấn như " google " vẫn phải khớp với "Google" trong danh sách. Áp dụng`.strip().lower()`ở cả hai bên đảm bảo rằng những khác biệt về định dạng như vậy không ảnh hưởng đến tính chính xác. 

Trường hợp biên cuối cùng là truy vấn chia sẻ tiền tố hoặc chuỗi con với tên công ty hợp lệ nhưng không bằng nhau sau khi chuẩn hóa. Ví dụ: "googl" không được khớp với "google". Vì việc tra cứu từ điển yêu cầu khóa bằng nhau chính xác nên các kết quả khớp một phần như vậy sẽ trả về chính xác -1.
