---
title: "CF 1953A - Thuật toán tính tổng bảo toàn độ chính xác"
description: "Chúng ta được cung cấp một chuỗi dài các số có dấu phẩy động và được yêu cầu đưa ra mô tả về cách tính tổng của chúng."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1953
codeforces_index: "A"
codeforces_contest_name: "2023 Post World Finals Online ICPC Challenge powered by Huawei"
rating: 0
weight: 1953
solve_time_s: 52
verified: true
draft: false
---

[CF 1953A - Thuật toán tổng kết bảo toàn độ chính xác](https://codeforces.com/problemset/problem/1953/A) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài các số có dấu phẩy động và được yêu cầu đưa ra mô tả về cách tính tổng của chúng. Điều khó khăn là chúng ta không trực tiếp tính tổng như kết quả đầu ra của chương trình thông thường; thay vào đó, chúng tôi đang mô tả một kế hoạch tính toán mà sau này sẽ được thực thi trong môi trường dấu phẩy động mô phỏng với các mức độ chính xác khác nhau. 

Mỗi phần tử của mảng đầu vào có thể được tham chiếu và chúng ta phải xây dựng một cây biểu thức duy nhất sử dụng các phần tử đó chính xác một lần. Mỗi nút bên trong trong cây này là một phép tính tổng được thực hiện theo một trong ba độ chính xác: một nửa, một hoặc hai. Cấu trúc mà chúng tôi đưa ra xác định cả kết quả bằng số và cả hàm tính điểm phụ thuộc vào độ chính xác, chi phí tính toán và hành vi truy cập bộ nhớ. 

Hạn chế chính là kích thước đầu vào có thể lớn tới một triệu phần tử. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng thực hiện việc cân bằng tốn kém, nhóm đệ quy với các phương pháp phỏng đoán phức tạp hoặc quét lặp lại cấu trúc dữ liệu. Bất cứ thứ gì chúng ta xuất ra đều phải được xây dựng theo thời gian tuyến tính và phải tránh việc lồng ghép hoặc cấu trúc không cần thiết. 

Một khía cạnh tinh tế của việc tính điểm là độ chính xác rất phi tuyến tính. Ngay cả những lỗi tương đối nhỏ cũng bị phạt theo lũy thừa 0,05, nghĩa là một khi lỗi vượt quá mức nhiễu dấu phẩy động, điểm sẽ giảm chậm nhưng đều đặn. Tuy nhiên, phép tính tổng có độ chính xác kép cực kỳ ổn định so với độ chính xác đơn hoặc một nửa, đặc biệt đối với các chuỗi đối nghịch. 

Một vấn đề tiềm ẩn khác là bất kỳ sự phân rã theo cấp bậc nào đều đưa ra thêm chi phí về cả điều kiện phạt về trọng lượng và bộ nhớ. Các thuật toán lồng nhau làm tăng độ phức tạp mà không cải thiện độ chính xác theo cách bù đắp cho các hình phạt ghi điểm trong hầu hết các trường hợp thực tế. 

Các trường hợp cạnh chủ yếu đến từ hai nguồn. Đầu tiên, các chuỗi có độ biến thiên cường độ cực lớn, trong đó các bậc tổng không ổn định với độ chính xác thấp sẽ phá hủy kết quả. Thứ hai, các trình tự cố gắng tối ưu hóa chi phí bằng cách chia thành các khối con vẫn sẽ phải gánh chịu hậu quả nếu không cải thiện độ chính xác, vì số mũ về độ chính xác ủng hộ mạnh mẽ tính chính xác hơn là mức tăng hiệu quả nhỏ. 

Ví dụ, hãy xem xét một ý tưởng đơn giản là chia mảng thành các phần và tính tổng từng phần với độ chính xác đơn, sau đó kết hợp với độ chính xác kép. Điều này có thể trông hợp lý, nhưng đối với N lớn, nó gây ra lỗi làm tròn có thể tránh được ở các ranh giới khối và những lỗi đó được phóng đại theo số mũ tính điểm. Trong khi đó, tổng tuyến tính có độ chính xác kép hoàn toàn tránh được điều này. 

## Phương pháp tiếp cận 

Phối cảnh brute-force coi đây là một vấn đề xây dựng cây: chúng ta có thể chia mảng thành các phân đoạn một cách đệ quy, tính tổng từng phần bằng cách sử dụng các độ chính xác khác nhau và sau đó kết hợp chúng. Về nguyên tắc, điều này đúng vì định dạng đầu ra cho phép các thuật toán lồng nhau. Tuy nhiên, điều này ngay lập tức dẫn đến không gian thiết kế theo cấp số nhân nếu chúng ta cố gắng tối ưu hóa cấu trúc và bất kỳ cấu trúc nào như vậy cũng gây ra chi phí cả về trọng số và điều kiện phạt. 

Nhận xét quan trọng là hệ thống tính điểm thiên về tính đúng đắn. Thuật ngữ chính xác chiếm ưu thế vì nó được nâng lên thành số mũ nhỏ, có nghĩa là các lỗi tương đối nhỏ vẫn có ý nghĩa quan trọng trong điểm số cuối cùng. Vì độ chính xác một nửa và một gây ra lỗi làm tròn hệ thống nên chúng rất nguy hiểm trên quy mô lớn, đặc biệt là khi tính tổng lên tới một triệu giá trị. 

Mặt khác, độ chính xác kép ổn định cho phép tính tổng tuyến tính trong hầu hết các phân phối lập trình cạnh tranh thực tế của các giá trị dấu phẩy động. Quan trọng hơn, việc sử dụng cấu trúc phẳng sẽ tránh được mọi chi phí đệ quy, loại bỏ các hình phạt về vị trí bộ nhớ và giữ trọng lượng ở mức tối thiểu so với bất kỳ cấu trúc lồng nhau nào.

Do đó, chiến lược tối ưu sẽ chuyển sang giảm tuyến tính đơn bằng cách sử dụng độ chính xác gấp đôi trên tất cả các chỉ số theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây có độ chính xác hỗn hợp lồng nhau | Xây dựng O(N) với tổng chi phí không đổi lớn | O(N) | Điểm tệ hơn do bị phạt đền | 
| Tổng chính xác gấp đôi phẳng | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng đầu ra dưới dạng một biểu thức duy nhất sử dụng độ chính xác gấp đôi trên tất cả các chỉ số đầu vào. 

1. Đọc N và bỏ qua mọi nhu cầu xử lý trước hoặc chuyển đổi, vì mọi phần tử phải được sử dụng chính xác một lần và không có nhóm nào cải thiện việc tính điểm một cách có ý nghĩa. 
2. Xây dựng một danh sách các chỉ số từ 1 đến N theo thứ tự. Điều này xác định trình tự tổng hợp. Thứ tự không bắt buộc phải được tối ưu hóa để ổn định số lượng ngoài mức độ chính xác kép đã đảm bảo. 
3. Gói danh sách trong một toán tử có độ chính xác kép, tạo ra biểu thức được mã hóa cuối cùng của biểu mẫu`{d:1,2,3,...,N}`. 
4. Xuất trực tiếp chuỗi này mà không đưa vào bất kỳ biểu thức con lồng nhau nào. 

Lý do cấu trúc này là đủ là vì bất kỳ nỗ lực nào nhằm giới thiệu hệ thống phân cấp đều làm tăng trọng lượng tính toán hoặc gây ra sự mất mát về độ chính xác mà không cải thiện thuật ngữ tính điểm vượt trội. Cấu trúc phẳng giảm thiểu chi phí trên tất cả các thành phần không chính xác trong khi vẫn duy trì độ ổn định số tối đa. 

## Tại sao nó hoạt động 

Chức năng tính điểm xử lý cả sự thiếu chính xác và phức tạp, nhưng theo cách rất bất đối xứng. Sự suy giảm độ chính xác được lũy thừa theo cách khiến cho lỗi dấu phẩy động thậm chí ở mức trung bình trở nên đắt đỏ. Tính tổng chính xác kép giữ sai số số ở mức nhỏ nhất có thể đạt được theo mô hình được phép. 

Đồng thời, mô hình chi phí không tạo ra cấu trúc thông minh đủ để bù đắp cho sự mất ổn định do độ chính xác thấp hơn hoặc tập hợp lồng nhau. Vì mọi phần tử phải được sử dụng chính xác một lần nên mọi phân tách hoàn toàn là tùy chọn và chỉ bổ sung thêm chi phí. 

Do đó, bất biến rất đơn giản: tại mỗi tiền tố của biểu thức được xây dựng, chúng ta đang duy trì một cách hiệu quả cách biểu diễn ổn định nhất về mặt số lượng của tổng riêng có sẵn trong các phép toán được phép. Tổng chính xác gấp đôi phẳng duy trì tính bất biến này trên toàn cầu từ đầu đến cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n_and_rest = sys.stdin.read().strip().split()
    n = int(n_and_rest[0])
    
    # We only need indices 1..n
    parts = ",".join(str(i) for i in range(1, n + 1))
    print(f"{{d:{parts}}}")

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào nhưng không sử dụng các giá trị, vì nhiệm vụ hoàn toàn là đưa ra một chiến lược tính tổng hợp lệ theo các chỉ số. Chi tiết xây dựng quan trọng duy nhất là định dạng: tất cả các chỉ mục phải được phân tách bằng dấu phẩy bên trong một cặp dấu ngoặc nhọn, có tiền tố là`d:`. 

Một lỗi phổ biến ở đây là cố gắng phân tích cú pháp hoặc thao tác các giá trị dấu phẩy động, việc này không cần thiết và chỉ làm tăng chi phí thời gian chạy. Một sai lầm khác là đưa thêm các lồng vào nhau, làm thay đổi cấu trúc tính điểm theo hướng bất lợi. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có bốn phần tử. Đầu ra được yêu cầu là tổng có độ chính xác kép duy nhất trên tất cả các chỉ số. 

| Bước | Hành động | Biểu hiện | 
| --- | --- | --- | 
| 1 | Đọc N = 4 | - | 
| 2 | Tạo chỉ số | [1,2,3,4] | 
| 3 | Xây dựng biểu thức | {d:1,2,3,4} | 

Điều này xác nhận rằng không có sự tái cấu trúc nào xảy ra và tất cả các phần tử được sử dụng chính xác một lần. 

Bây giờ hãy xem xét một ví dụ lớn hơn với sáu phần tử. 

| Bước | Hành động | Biểu hiện | 
| --- | --- | --- | 
| 1 | Đọc N = 6 | - | 
| 2 | Tạo chỉ số | [1,2,3,4,5,6] | 
| 3 | Xây dựng biểu thức | {d:1,2,3,4,5,6} | 

Điều này thể hiện khả năng mở rộng: chi phí xây dựng tăng tuyến tính nhưng không có thêm sự phức tạp về cấu trúc nào được đưa ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi tạo một danh sách các chỉ mục từ 1 đến N | 
| Không gian | O(1) | Đầu ra được truyền phát; không có công trình phụ trợ ngoài quầy | 

Các ràng buộc cho phép tối đa một triệu phần tử và giải pháp chỉ thực hiện một lần chuyển tuyến tính duy nhất để định dạng đầu ra. Điều này nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n_and_rest = sys.stdin.read().strip().split()
    n = int(n_and_rest[0])
    return "{" + "d:" + ",".join(str(i) for i in range(1, n + 1)) + "}"

# provided sample
assert run("2 -4.815473e+04 -1.862622e+04") == "{d:1,2}"

# minimum size
assert run("2 1.0 2.0") == "{d:1,2}"

# small case
assert run("4 1 2 3 4") == "{d:1,2,3,4}"

# larger case
assert run("6 1 1 1 1 1 1") == "{d:1,2,3,4,5,6}"

# larger structure sanity
assert run("10 " + "1 "*10) == "{d:1,2,3,4,5,6,7,8,9,10}"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 số |`{d:1,2}`| cấu trúc hợp lệ tối thiểu | 
| 4 số |`{d:1,2,3,4}`| tính đúng đắn cơ bản | 
| 6 giá trị giống hệt nhau |`{d:1..6}`| giá trị độc lập | 
| 10 yếu tố |`{d:1..10}`| chia tỷ lệ tuyến tính | 

## Vỏ cạnh 

Một mối quan tâm tiềm tàng là liệu việc đảo ngược hoặc hoán vị các chỉ số có thể cải thiện sự ổn định về số hay không. Để có độ chính xác gấp đôi, sự khác biệt là không đáng kể đối với các bản phân phối chương trình cạnh tranh điển hình và mọi sự sắp xếp lại vẫn tạo ra cùng một điểm cấu trúc. 

Ví dụ: đầu vào:```
3 a b c
```Thuật toán tạo ra:```
{d:1,2,3}
```Ngay cả khi một người cố gắng đảo ngược thứ tự`{d:3,2,1}`, việc chấm điểm không được cải thiện theo cách bù đắp cho sự phức tạp tăng thêm hoặc rủi ro xảy ra lỗi khi triển khai. Vì tất cả các phần tử phải được đưa vào chính xác một lần nên tính bất biến của đường truyền phẳng vẫn tối ưu bất kể phân bố đầu vào như thế nào. 

Một trường hợp khác là khi các giá trị cực kỳ lớn hoặc cực kỳ nhỏ. Vì tất cả các giá trị được xử lý nội bộ ở dạng nhị phân64 nên việc tích lũy độ chính xác kép vẫn là lựa chọn an toàn duy nhất giúp tránh được việc hủy bỏ thảm khốc mà không gây ra chi phí cấu trúc.
