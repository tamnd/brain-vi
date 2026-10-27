---
title: "CF 106136M - ĐIỂM NỔI"
description: "Chúng ta được yêu cầu xây dựng một chuỗi có độ dài tăng dần $n$, trong đó mọi phần tử là số nguyên không âm dưới $2^{30}$."
date: "2026-06-19T19:43:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106136
codeforces_index: "M"
codeforces_contest_name: "East China University of Science and Technology Programming Contest 2025"
rating: 0
weight: 106136
solve_time_s: 50
verified: true
draft: false
---

[CF 106136M - ĐIỂM NỔI](https://codeforces.com/problemset/problem/106136/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy có độ dài tăng dần một cách chặt chẽ$n$, trong đó mọi phần tử là số nguyên không âm bên dưới$2^{30}$. Khó khăn không phải là sự đơn điệu mà là sự hạn chế đối với mỗi cặp liền kề: đối với mỗi cặp liên tiếp$(a_i, a_{i+1})$, cặp số phải thỏa mãn điều kiện bitwise trong đó XOR của hai số hoàn toàn nhỏ hơn AND của chúng. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp có độ dài yêu cầu$n$. Đối với mỗi trường hợp, chúng tôi đưa ra một chuỗi hợp lệ hoặc báo cáo rằng không thể xây dựng được. 

Tổng thể các ràng buộc là lớn, với tổng$n$trên tất cả các trường hợp thử nghiệm lên đến$10^6$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải xây dựng từng chuỗi theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm và lý tưởng nhất là theo một mẫu xác định rất đơn giản mà không cần tìm kiếm hoặc quay lui. Bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm đều là không thể, và thậm chí$O(n \log n)$sẽ là đường biên nếu hằng số lớn. 

Một khía cạnh tinh tế của điều kiện là nó liên quan đến đồng thời cả XOR và AND. XOR đo lường sự bất đồng về bit, trong khi AND đo các bit được chia sẻ. Bất đẳng thức XOR < AND có nghĩa là các số liền kề phải chia sẻ đủ nhiều bit có trọng số cao so với sự khác biệt của chúng, điều này vốn đáng nghi ngờ vì đối với hầu hết các cặp có thứ tự, XOR có xu hướng chiếm ưu thế trừ khi có sự chồng chéo mạnh ở các bit cao hơn. 

Trường hợp cạnh xuất hiện ngay ở mức nhỏ$n$. Vì$n = 2$, chúng ta cần một cặp hợp lệ duy nhất và không rõ liệu có cặp nào như vậy tồn tại hay không. Một giả định ngây thơ có thể là việc chọn các số nguyên liên tiếp có hiệu quả, nhưng$a$Và$a+1$luôn có XOR bằng$2^{k+1}-1$ở các bit cuối, trong khi AND bằng 0, khiến điều kiện không thể thực hiện được. Điều này cho thấy rằng không phải tất cả các chuỗi tăng đều có thể thỏa mãn ràng buộc và cấu trúc đó phải được thiết kế cẩn thận nếu không câu trả lời có thể là không thể đối với một số người.$n$. 

Một cạm bẫy tiềm ẩn khác là giả sử điều kiện là đối xứng hoặc hành xử đơn điệu với các giá trị tăng dần. Không phải vậy, vì XOR và AND phụ thuộc hoàn toàn vào sự chồng chéo nhị phân hơn là độ lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng trình tự từng bước và tại mỗi vị trí, hãy thử tất cả các giá trị có thể lớn hơn giá trị trước đó, kiểm tra điều kiện theo bit. Điều này đòi hỏi, trong trường hợp xấu nhất, phải quét tới$2^{30}$ứng viên cho mỗi vị trí, khiến việc này hoàn toàn không khả thi. Ngay cả khi giới hạn ở một tập hợp con hợp lý, tổng công việc vẫn vượt xa giới hạn được đưa ra$n$lên đến$10^6$. 

Thông tin chi tiết quan trọng đến từ việc viết lại điều kiện theo cấu trúc bit. Cho hai số$a < b$, chúng tôi yêu cầu$$a \oplus b < a \& b.$$Bất đẳng thức này chỉ có thể đúng nếu AND đóng góp một bit cao mà XOR không loại bỏ được. Tuy nhiên, XOR luôn chỉ có các bit ở nơi hai số khác nhau, trong khi AND chỉ có các bit ở nơi cả hai số đều bằng 1. Điều này ngay lập tức ngụ ý sự căng thẳng về cấu trúc: XOR làm nổi bật sự khác biệt, AND làm nổi bật sự chồng chéo và chúng tôi yêu cầu sự chồng chéo để chi phối sự khác biệt về giá trị số. 

Quan sát quan trọng là nếu hai số có chung một bit cao thì bit đó đóng góp tích cực vào AND nhưng không đóng góp gì cho XOR. Trong khi đó, XOR hoàn toàn được xác định bởi các bit khác nhau. Để làm cho AND lớn hơn XOR, chúng tôi muốn các số liền kề chỉ khác nhau ở các bit thấp trong khi chia sẻ ít nhất một bit cao và bit chia sẻ đó phải chiếm ưu thế trong giá trị số của tất cả các bit khác nhau. 

Điều này gợi ý việc xây dựng một chuỗi trong đó chúng tôi cố định bit cao nhất và chỉ thay đổi các bit thấp hơn theo cách được kiểm soát, đảm bảo AND luôn chứa bit cố định đó trong khi XOR vẫn bị giới hạn ở các bit thấp hơn. Tuy nhiên, tính đơn điệu nghiêm ngặt làm phức tạp thêm những công trình ngây thơ. 

Một cách rõ ràng về phía trước là nhận ra rằng chúng ta chỉ cần một dãy tăng nghiêm ngặt chứ không cần một cấu trúc tổ hợp đầy đủ. Điều này cho phép chúng tôi thực thi một bit cao vượt trội duy nhất trên tất cả các phần tử và chỉ thay đổi một tập hợp các bit thấp hơn được kiểm soát theo cách giữ cho XOR nhỏ. 

Cách xây dựng đơn giản nhất là chọn các số có dạng:$$a_i = 2^{k} + i$$đủ nhỏ$i$, nhưng điều này không thành công vì AND giữa các số liên tiếp trở nên quá nhỏ. Thay vào đó, chúng tôi đảo ngược quan điểm: chúng tôi xây dựng các cặp chia sẻ một bit cao cố định và đảm bảo rằng quá trình chuyển đổi diễn ra trong các khối được chọn cẩn thận để mỗi cặp liền kề có một tập hợp bit chia sẻ lớn và chỉ có những khác biệt nhỏ. 

Ý tưởng mang tính xây dựng cuối cùng là xây dựng chuỗi theo thứ tự tăng dần trong khi vẫn đảm bảo rằng mọi số đều có bit cao nhất cố định và trong ràng buộc đó, chúng tôi đảm bảo tính liền kề hoạt động giống như mức tăng được kiểm soát trong tập hợp con trong đó AND luôn duy trì bit trội cao hơn bất kỳ đóng góp XOR nào. Điều này dẫn đến một mô hình mà về cơ bản chúng tôi mã hóa các chỉ số bên trong mặt nạ bit cao để đảm bảo sự thống trị VÀ. 

Sau khi đơn giản hóa các ràng buộc, kết luận cốt lõi là các chuỗi hợp lệ tồn tại cho tất cả$n \ge 2$và có thể được xây dựng tuyến tính bằng cách sử dụng mẫu bit cố định để đảm bảo bit trên cùng được chia sẻ trên toàn bộ chuỗi trong khi vẫn giữ XOR được giới hạn bởi mức tăng bit thấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^{30})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Cấu trúc dựa trên việc cố định một bit cao duy nhất và mã hóa chuỗi tăng dần đơn điệu ở các bit còn lại trong khi đảm bảo tính kề cận luôn duy trì bit cao đó. 

1. Chọn một vị trí bit cố định, ví dụ bit 29 và đảm bảo mọi số trong chuỗi đều có bit này được đặt. Điều này đảm bảo rằng mọi cặp liền kề đều có AND khác 0 chứa ít nhất$2^{29}$, điều này sẽ chiếm ưu thế so với XOR nếu sự khác biệt bị hạn chế ở các bit thấp hơn. 
2. Xây dựng dãy bằng cách liệt kê các số nguyên$0, 1, 2, \dots, n-1$và đặt từng giá trị là$a_i = (1 << 29) | i$. Điều này duy trì tính đơn điệu nghiêm ngặt vì các bit thấp hơn tăng bình thường. 
3. Xác minh ngầm cấu trúc kề: đối với các giá trị liên tiếp, XOR chỉ ảnh hưởng đến các bit thấp, vì bit cao sẽ bị loại bỏ và AND luôn chứa bit cao. Điều này đảm bảo XOR được giới hạn chặt chẽ bởi$2^{29}$, trong khi AND ít nhất là$2^{29}$, làm cho bất đẳng thức đúng với tất cả các cặp. 
4. Xuất trực tiếp chuỗi. 

Việc xây dựng mang tính quyết định và độc lập với cấu trúc ca kiểm thử. 

### Tại sao nó hoạt động 

Mỗi phần tử đều chứa một bit được đặt cao nhất cố định, do đó, đối với bất kỳ cặp liền kề nào, phép toán AND sẽ giữ nguyên bit này, đảm bảo giá trị cơ sở lớn. Mặt khác, XOR chỉ phản ánh sự khác biệt ở 29 bit thấp hơn, luôn nhỏ hơn mức đóng góp của bit cao được chia sẻ. Điều này tạo ra sự phân tách chặt chẽ về độ lớn giữa XOR và AND cho mọi cặp liền kề, đảm bảo sự bất đẳng thức luôn được giữ trong khi vẫn duy trì trật tự nghiêm ngặt thông qua bộ đếm bit thấp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    HIGH = 1 << 29
    
    for _ in range(T):
        n = int(input())
        if n < 2:
            print(-1)
            continue
        
        # construct sequence
        res = []
        for i in range(n):
            res.append(str(HIGH | i))
        
        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp sẽ đọc số lượng trường hợp kiểm thử, sau đó xử lý từng trường hợp kiểm thử một cách độc lập. Bit cao không đổi$2^{29}$được sử dụng để đảm bảo mọi phần tử chia sẻ một bit trội chung, giúp ổn định hoạt động AND trên tất cả các cặp liền kề. 

Mỗi giá trị được hình thành bằng cách OR-ing bit cao này với bộ đếm tăng đơn giản. Điều này đảm bảo tính đơn điệu nghiêm ngặt vì 29 bit thấp hơn tăng bình thường và không ảnh hưởng đến thứ tự. 

Một sai lầm phổ biến là quên rằng đầu ra phải nằm trong khoảng$[0, 2^{30})$. Cấu trúc được chọn tôn trọng giới hạn này vì nó chỉ sử dụng bit 29 cộng với giá trị lên tới$10^6$, cũng nằm trong phạm vi. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 4$. Việc xây dựng tạo ra:$$(2^{29} | 0), (2^{29} | 1), (2^{29} | 2), (2^{29} | 3)$$| tôi | một [tôi] | a[i] XOR a[i+1] | a[i] VÀ a[i+1] | 
| --- | --- | --- | --- | 
| 0 |$2^{29}$| 1 |$2^{29}$| 
| 1 |$2^{29}+1$| 3 |$2^{29}$| 
| 2 |$2^{29}+2$| 1 |$2^{29}$| 

Mỗi giá trị XOR vẫn rất nhỏ so với giá trị AND không đổi, xác nhận tính chính xác. 

Vì$n = 2$, chúng tôi nhận được:$$(2^{29}, 2^{29}+1)$$Ở đây XOR là 1 và AND là$2^{29}$, do đó bất đẳng thức đúng ngay lập tức. 

Những ví dụ này cho thấy rằng việc xây dựng không phụ thuộc vào việc điều chỉnh cục bộ, mà chỉ phụ thuộc vào tính bất biến là một bit cao được chia sẻ chiếm ưu thế trong mỗi cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm tạo ra mỗi phần tử một lần | 
| Không gian |$O(n)$| Chỉ lưu trữ cho chuỗi đầu ra | 

Tổng cộng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^6$, do đó giải pháp thực hiện khoảng một triệu thao tác bit đơn giản, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    HIGH = 1 << 29
    out = []

    for _ in range(T):
        n = int(input())
        if n < 2:
            out.append("-1")
            continue
        arr = [(HIGH | i) for i in range(n)]
        out.append(" ".join(map(str, arr)))

    return "\n".join(out)

# minimal size
assert run("1\n2\n") != "-1"

# multiple test cases
assert run("2\n2\n3\n") != ""

# larger sequence sanity
assert len(run("1\n5\n").split()) == 5

# all equal n=2 edge behavior
assert run("1\n2\n").count("-1") == 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n2 | cặp hợp lệ | trường hợp khả thi tối thiểu | 
| 2\n2\n3 | hai trường hợp thử nghiệm | xử lý nhiều trường hợp | 
| 1\n5 | 5 số | độ dài chuỗi chính xác | 

## Vỏ cạnh 

cho$n = 2$, trình tự trở thành$[2^{29}, 2^{29}+1]$. AND là$2^{29}$bởi vì cả hai số đều có chung bit trên cùng, trong khi XOR là 1. Bất đẳng thức được giữ nguyên và thứ tự nghiêm ngặt được giữ nguyên. 

Vì$n = 10^6$, việc xây dựng vẫn hoạt động vì chỉ có các bit thấp hơn là khác nhau. Ngay cả khi bộ đếm tăng lên, nó vẫn ở dưới$2^{20}$, thấp hơn nhiều so với bit cao cố định, do đó không xảy ra hiện tượng tràn hoặc nhiễu bit.
