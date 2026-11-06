---
problem: 1281A
contest_id: 1281
problem_index: A
name: "Suffix Three"
contest_name: "Codeforces Round 607 (Div. 2)"
rating: 800
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 147
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2da53e-aedc-83ec-a672-425fbbff0c7c
---

#CF 1281A - Hậu Tố Ba 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2da53e-aedc-83ec-a672-425fbbff0c7c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều câu độc lập. Mỗi câu là một chuỗi trong đó các từ được nối bằng dấu gạch dưới thay vì dấu cách. Nhiệm vụ của chúng ta là phân loại từng câu thành một trong ba ngôn ngữ bằng cách chỉ xem câu kết thúc như thế nào. 

Quy tắc phân loại hoàn toàn dựa trên hậu tố. Nếu câu kết thúc bằng một số mẫu cố định nhất định, chúng tôi sẽ ánh xạ nó tới tên ngôn ngữ. Cấu trúc câu trước khi kết thúc không quan trọng chút nào, chỉ có ký tự cuối cùng mới quyết định câu trả lời. 

Các ràng buộc rất nhỏ: tối đa 30 câu, mỗi câu tối đa 1000 ký tự. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào kiểm tra từng ký tự hoặc thực hiện kiểm tra hậu tố theo thời gian không đổi cho mỗi trường hợp kiểm thử đều dễ dàng đủ nhanh. Ngay cả một cách tiếp cận đơn giản để kiểm tra tất cả các kết quả phù hợp với hậu tố có thể có trên mỗi chuỗi cũng không đáng kể trong các giới hạn này vì tổng công việc bị giới hạn bởi khoảng 30.000 ký tự. 

Một trường hợp tinh vi cần cẩn thận là đảm bảo chúng ta chỉ khớp các hậu tố ở cuối chuỗi chứ không vô tình ở bên trong chuỗi. Ví dụ: một tìm kiếm đơn giản cho "po" ở bất cứ đâu sẽ sai vì chỉ có phần kết thúc mới quan trọng. Một cạm bẫy tiềm ẩn khác là độ dài hậu tố chồng chéo. Ví dụ: "masu" và "desu" đều biểu thị tiếng Nhật, do đó việc triển khai phải kiểm tra chính xác cả hai mà không giả sử một độ dài hậu tố cố định duy nhất. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là quét từng câu và kiểm tra xem nó có kết thúc bằng bất kỳ hậu tố nào đã biết hay không bằng cách so sánh các ký tự ở cuối theo cách thủ công. Đối với mỗi chuỗi, chúng ta có thể so sánh nó với tất cả các hậu tố bằng cách cắt và kiểm tra tính bằng nhau. Vì chỉ có bốn ứng cử viên hậu tố, phương pháp này thực hiện tối đa bốn phép so sánh cho mỗi trường hợp kiểm thử, mỗi phép so sánh lấy O(L) trong trường hợp xấu nhất trong đó L là độ dài hậu tố. Với L giới hạn bởi 1000, giá trị này vẫn cực kỳ nhỏ. 

Quan sát chính là chúng ta không cần bất kỳ quá trình tiền xử lý hoặc phân tích cú pháp phức tạp nào. Vấn đề hoàn toàn nằm ở việc kiểm tra xem chuỗi có kết thúc bằng một trong số ít mẫu cố định hay không. Điều đó có nghĩa là chúng ta có thể trực tiếp sử dụng các phép toán so sánh hậu tố. Cấu trúc của đầu vào đảm bảo rằng chính xác một hậu tố khớp với nhau, vì vậy chúng ta không cần xử lý sự mơ hồ hoặc các quy tắc ưu tiên. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu về cơ bản là thừa nhận rằng không gian quyết định có kích thước không đổi. Thay vì quét hoặc phân tích cú pháp, chúng tôi giảm từng trường hợp thử nghiệm thành một số so sánh chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét thủ công) | O(t · L) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra hậu tố) | O(t · L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng câu một cách độc lập và xác định ngôn ngữ của câu đó bằng cách kiểm tra hậu tố. 

1. Đọc số lượng test case. Điều này xác định có bao nhiêu hoạt động phân loại độc lập mà chúng tôi sẽ thực hiện. 
2. Đối với mỗi câu, hãy kiểm tra xem nó có kết thúc bằng hậu tố "mnida" hay không. Nếu có, xuất ra "HÀN QUỐC". Việc kiểm tra này được thực hiện trước tiên vì nó là duy nhất và tránh sự mơ hồ không cần thiết với các hậu tố khác. 
3. Nếu nó không kết thúc bằng "mnida", hãy kiểm tra xem nó kết thúc bằng "desu" hay "masu". Nếu một trong hai điều kiện được giữ, hãy xuất ra "NHẬT BẢN". Hai hậu tố này thuộc cùng một nhóm ngôn ngữ nên chúng được kết hợp một cách hợp lý bằng điều kiện OR. 
4. Nếu cả hai điều trên đều không khớp thì câu phải kết thúc bằng "po", do đó xuất ra "FILIPINO". Sự cố đảm bảo rằng mọi đầu vào đều kết thúc bằng một trong các hậu tố hợp lệ, do đó không cần xác thực dự phòng.

Thứ tự kiểm tra không thực sự cần thiết trong vấn đề này vì các hậu tố không trùng lặp theo cách gây ra sự mơ hồ, nhưng việc viết chúng một cách rõ ràng sẽ giữ cho logic rõ ràng và tránh việc phân loại sai ngẫu nhiên nếu các quy tắc được mở rộng. 

### Tại sao nó hoạt động 

Mỗi câu được đảm bảo kết thúc bằng chính xác một hậu tố hợp lệ từ một tập hợp cố định. Vì đẳng thức hậu tố là thuộc tính xác định nên việc kiểm tra trực tiếp các ký tự cuối cùng là đủ để nhận dạng duy nhất ngôn ngữ. Thuật toán dựa trên tính bất biến rằng không có câu hợp lệ nào có thể khớp với nhiều hơn một hậu tố ngôn ngữ ở cuối, vì vậy điều kiện khớp đầu tiên luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()

    if s.endswith("mnida"):
        print("KOREAN")
    elif s.endswith("desu") or s.endswith("masu"):
        print("JAPANESE")
    else:
        print("FILIPINO")
```Giải pháp sử dụng tích hợp sẵn của Python`endswith`phương pháp trực tiếp thực hiện so sánh hậu tố một cách hiệu quả. Mỗi lần kiểm tra chạy theo thời gian tỷ lệ thuận với độ dài hậu tố, không đổi và rất nhỏ. 

Thứ tự của các điều kiện phản ánh sự nhóm hợp lý của các hậu tố. Tiếng Hàn được chọn đầu tiên vì đây là danh mục hậu tố đơn riêng biệt. Tiếp theo, tiếng Nhật sẽ được xử lý bằng cách sử dụng điều kiện OR kết hợp. Filipino trở thành trường hợp mặc định vì vấn đề đảm bảo tính đầy đủ của tập hậu tố. 

Việc sử dụng`strip()`đảm bảo rằng các ký tự dòng mới không ảnh hưởng đến việc khớp hậu tố, đây là lỗi chi tiết triển khai phổ biến trong cài đặt cuộc thi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện để xem cách phát hiện hậu tố xác định đầu ra. 

### Ví dụ 1 

đầu vào:```
kamusta_po
genki_desu
```| Bước | Chuỗi | Kết thúc bằng "mnida"? | Kết thúc bằng "desu"/"masu"? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | kamusta_po | Không | Không | PHILIPIN | 
| 2 | genki_desu | Không | Có | NHẬT BẢN | 

Chuỗi đầu tiên kết thúc bằng "po", do đó chuỗi này không thực hiện được tất cả các lần kiểm tra trước đó và mặc định là tiếng Philippin. Chuỗi thứ hai khớp với hậu tố tiếng Nhật "desu", kích hoạt điều kiện thứ hai. 

### Ví dụ 2 

đầu vào:```
annyeong_hashimnida
si_roy_mustang_ay_namamasu
```| Bước | Chuỗi | Kết thúc bằng "mnida"? | Kết thúc bằng "desu"/"masu"? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | annyeong_hashinida | Có | - | HÀN QUỐC | 
| 2 | si_roy_mustang_ay_namamasu | Không | Có | NHẬT BẢN | 

Câu đầu tiên khớp rõ ràng với hậu tố tiếng Hàn "mnida", xác định ngay kết quả đầu ra mà không cần kiểm tra thêm. Trận thứ hai là "masu", xác nhận tiếng Nhật. 

Những ví dụ này xác nhận rằng chỉ cần kết hợp hậu tố là đủ và rõ ràng theo các đảm bảo nhất định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · L) | Mỗi trường hợp thử nghiệm thực hiện một số lượng so sánh hậu tố không đổi, mỗi lần quét tối đa một vài ký tự cuối cùng của chuỗi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 30 chuỗi có độ dài 1000, do đó, tối đa 30.000 kiểm tra ký tự được thực hiện, điều này không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        if s.endswith("mnida"):
            out.append("KOREAN")
        elif s.endswith("desu") or s.endswith("masu"):
            out.append("JAPANESE")
        else:
            out.append("FILIPINO")
    return "\n".join(out)

# provided sample
assert run("""8
kamusta_po
genki_desu
ohayou_gozaimasu
annyeong_hashimnida
hajime_no_ippo
bensamu_no_sentou_houhou_ga_okama_kenpo
ang_halaman_doon_ay_sarisari_singkamasu
si_roy_mustang_ay_namamasu
""") == """FILIPINO
JAPANESE
JAPANESE
KOREAN
FILIPINO
FILIPINO
JAPANESE
JAPANESE"""

# custom: single character string edge (invalid but structurally safe)
assert run("""1
po
""") == "FILIPINO"

# custom: all Japanese variants
assert run("""2
a_desu
b_masu
""") == """JAPANESE
JAPANESE"""

# custom: Korean dominance check
assert run("""1
xmnida
""") == "KOREAN"

# custom: mixed unrelated prefix, Filipino fallback
assert run("""1
very_long_sentence_about_nothing_po
""") == "FILIPINO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`po`| PHILIPIN | hậu tố tiếng Philipin hợp lệ tối thiểu | 
|`a_desu / b_masu`| NHẬT BẢN | cả hai biến thể hậu tố tiếng Nhật | 
|`xmnida`| HÀN QUỐC | Tính chính xác của việc phát hiện hậu tố tiếng Hàn | 
| dài`_po`câu | PHILIPIN | tính chính xác của hậu tố độc lập với tiền tố | 

## Vỏ cạnh 

Trường hợp một cạnh là khi câu cực kỳ ngắn và chỉ bao gồm hậu tố. Ví dụ, đầu vào`po`vẫn nên được phân loại chính xác là người Philippines. Thuật toán xử lý việc này một cách tự nhiên vì`endswith("po")`đúng ngay cả khi toàn bộ chuỗi chỉ có hai ký tự. 

Một trường hợp khác là khi nhiều mẫu hậu tố có thể xuất hiện dưới dạng chuỗi con trước đó trong câu. Ví dụ,`po_desu_mnida_po`chứa tất cả ba mã thông báo bên trong, nhưng chỉ có hậu tố cuối cùng mới quan trọng. Việc triển khai là đúng vì nó không bao giờ quét toàn bộ chuỗi, chỉ quét phần đuôi. 

Trường hợp cạnh cuối cùng là độ dài hậu tố chồng chéo. Từ`"desu"`Và`"masu"`chia sẻ cùng một ngôn ngữ nhưng có kết thúc khác nhau, cả hai đều phải được kiểm tra rõ ràng. Điều kiện OR đảm bảo cả hai đều được xử lý như nhau và không phát sinh vấn đề về quyền ưu tiên vì chỉ có kết quả khớp cuối cùng mới quan trọng.
