# Giải mã mã nguồn của vắc-xin SARS-CoV-2 từ BioNTech/Pfizer


Xin chào! Trong bài này chúng ta sẽ phân tích chi tiết toàn bộ mã nguồn của vắc-xin mRNA SARS-CoV-2 tới từ BioNTech/Pfizer.

> Mình không phải tác giả mà dịch từ bản gốc tiếng anh [ở đây](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). Background của mình không phải là biotech nên trong quá trình dịch không thể tránh khỏi sai sót. Mọi đóng góp xin liên hệ với mình qua facebook [Ethan Phan](https://www.facebook.com/ethan.phan.79230/)

Điều này có thể nghe hơi lạ, vắc-xin là thứ chất lỏng để tiêm vào người thì tại sao chúng ta lại đan nói về mã nguồn ở đây?

Đây là một câu hỏi rất hay, hãy bắt đầu bằng việc nhìn vào một phần nhỏ của mã nguồn của vắc-xin từ BioNTech/Pfizer, được biết tới dưới cái tên BNT162b2, các tên gọi khác bao gồm Tozinameran hay Comirnaty.

<p align="center">
<img width="700" src="https://berthub.eu/articles/bnt162b2.png" alt="500 ký tư đầu tiên trong mã nguồn của BNT162b2">
</p>
<p align="center"> 500 ký tự đầu tiền trong mã nguồn của vắc-xin BNT162b2. Nguồn: <a href="https://mednet-communities.net/inn/db/media/docs/11889.doc">World Health Organization</a> </p>

Vắc-xin mRNA BNT162b có thành phần quan trọng nhất là một bộ mã nguồn kỹ thuật số dài 4284 ký tự, khá ngắn gọn để nằm vừa trong vài facebook post. Trong giai đoạn đầu tiền của quá trình sản xuất vắc-xin, đoạn mã nguồn này sẽ được đưa vào một máy in DNA (vâng, bạn không nghe nhầm đâu, máy in DNA là có thật đó), thứ sẽ dịch đoạn mã này thành những phân tử DNA thực sự.
<p align="center">
<img width="400" src="https://berthub.eu/articles/bioxp-3200.jpg" alt="Máy in DNA BioXp 3200">
</p>
<p align="center"> Máy in DNA BioXp 3200 đến từ <a href="https://codexdna.com/products/bioxp-system/">Codex DNA</a> </p>

Máy in sẽ tạo ra một lượng DNA rất nhỏ, thứ mà sau nhiều bước xử lý sinh học và hoá học sẽ biến thành RNA (sẽ nói thêm về RNA sau) trong các lọ vắc-xin. Một liều 30 microgram hoá ra chứa đúng 30 microgram RNA. Ngoài ra còn có một hệ thống đóng gói chất béo rất thông minh để đưa mRNA vào trong tế bào người.

RNA là phiên bản "bộ nhớ làm việc" dễ bay hơi (volatile ‘working memory’) của DNA. DNA giống như là chiếc ổ cứng sinh học vậy. DNA rất bền bỉ, dung lượng lớn đồng thời cũng rất tin cậy. Nhưng cũng giống như những chiếc máy tính không thể thực thi lệnh từ ổ cứng, trước khi được thực thi, lệnh phải được copy vào một bộ nhớ nhanh hơn, mềm dẻo hơn nhưng cũng mong manh hơn nhiều.

Trong máy tính, đó là RAM, trong sinh học là RNA. Chúng tương đồng một cách đáng kinh ngạc. Không giống như ổ cứng, dữ liệu trong RAM rất dễ biến mất nếu không được "chăm sóc" thường xuyên. Lý do mà vắc-xin mRNA của Pfizer/BioNTech phải được bảo quản ở nhiệt độ rất thấp cũng vậy: RNA là một đoá hoa mỏng manh.

Mỗi ký tự trong RNA nặng khoảng 0.53·10⁻²¹ gram, nghĩa là có khoảng 6·10¹⁶ ký tự trong một liều vắc-xin 30 microgram. Tính theo bytes thì là khoảng 25 petabytes, tuy nhiên nó vẫn chỉ là đoạn mã 4284 ký tự được copy tầm 2000 tỷ lần. Do đó lượng thông tin thực sự của vắc-xin chỉ khoảng hơn 1 kilobyte mà thôi. Bản thân vi rút SARS-CoV-2 chứa khoảng 7.5 kilobyte thông tin.

## Một chút thông tin thêm về DNA và RNA

DNA mà một đoạn mã kỹ thuật số. Không giống như trong những chiếc máy tính dùng 0 và 1, DNA dùng các ký tự A, C, G và U/T (các ‘nucleotides’, ‘nucleosides’ hay ‘bases).

A, C, G và U/T là các phân tử được lưu trữ dưới dạng chuỗi trong DNA (hoặc RNA).

Trong máy tính, mỗi nhóm 8 bít được nhóm lại thành 1 byte, và dữ liệu thường được xử lý theo từng byte.

Tự nhiên lại nhóm 3 nucleotides thành 1 condon (mình không biết tiếng việt cho mấy cái này), và condon thường là đơn vị xử lý cơ bản. Một condon chứa 6 bit dữ liệu (2 bit trên mỗi ký tự, 3 ký tự một condon => 6 bit. Đồng nghĩa với việc một codon có thể có có 2⁶ = 64 giá trị khác nhau).

Khá là kỹ thuật số phải không? Nếu còn thắc mắc về vấn đề này bạn có thể tra khảo thêm thông tin từ [tài liệu của WHO](https://mednet-communities.net/inn/db/media/docs/11889.doc).
> Một số thông tin chi tiết thêm có thể được tìm thấy [ở đây](https://berthub.eu/articles/posts/what-is-life/) - Đường link này ('Cuộc sống là gì') có thể giúp hiểu hơn về phần còn lại của bài này. Nếu bạn thích video, đây là [video dài 2 tiếng từ tác giả](https://berthub.eu/dna).

## Vậy cái đoạn mã đó nó làm gì?

Ý tưởng chung của vắc-xin là dạy cho hệ miễn dịch cách để chống lại các mầm bệnh mà không cần phải nhiễm các mầm bệnh đó. Việc này từ trước cho tới nay được thực hiện bằng cách tiêm vào trong cơ thể vi rút đã được làm yếu hoặc vô hiệu hoá (bất hoạt), cộng với một chất bổ trợ để kích thích hệ miễn dịch phản ứng. Cách này cần tới hàng tỷ quả trứng (hoặc côn trùng) để sản xuất ra vắc-xin. Cách này cũng tốn rất nhiều thời gian và đoi hỏi nhiều sự may mắn. Đôi khi cả một chủng vi rút không liên quan gì tới căn bệnh cũng được sử dụng.

Vắc-xin mRNA đạt được mục tiêu tương tự ('dạy cho hệ miễn dịch cách chống lại mầm bệnh') nhưng theo một cách như là tia laser vậy: rất tập trung nhưng cũng rất mạnh mẽ.

Và đây là cách nó hoạt động. Liều vắc-xin mà chúng ta đang nói tới chứa mRNA (hay messenger RNA) miêu tả phân tử protein 'Gai' (phân tử protein này nhìn như cái gai nằm trên mặt tế bào vi rút vậy) của vi rút SARS-CoV-2. Thông qua các phương tiện hoá học, vắc-xin sẽ đưa mRNA này vào trong tế bào người.

Điều này sau đó sẽ kích hoạt quá trình sản xuất protein 'Gai' của 
