package com.hanzala.musiclist

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
        //code starts here

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // Link views
            val edtSongTitle = findViewById<EditText>(R.id.edtSongTitle)
            val edtArtistName = findViewById<EditText>(R.id.edtArtistName)
            val edtRating = findViewById<EditText>(R.id.edtRating)
            val edtComments = findViewById<EditText>(R.id.edtComments)

            val btnAdd = findViewById<Button>(R.id.btnAddToPlayList)
            val btnView = findViewById<Button>(R.id.btnViewPlayList)
            val btnExit = findViewById<Button>(R.id.btnExitApp)

            // Add to playlist
            btnAdd.setOnClickListener {
                val title = edtSongTitle.text.toString()
                val artist = edtArtistName.text.toString()
                val ratingStr = edtRating.text.toString()
                val comment = edtComments.text.toString()

                if (title.isBlank() || artist.isBlank() || ratingStr.isBlank()) {
                    Toast.makeText(this, "Please fill in all fields.", Toast.LENGTH_SHORT).show()
                    return@setOnClickListener
                }

                val rating = ratingStr.toIntOrNull()
                if (rating == null || rating !in 1..5) {
                    Toast.makeText(this, "Rating must be a number from 1 to 5.", Toast.LENGTH_SHORT)
                        .show()
                    return@setOnClickListener
                }

                // Save data
                PlaylistData.songTitles.add(title)
                PlaylistData.artistNames.add(artist)
                PlaylistData.ratings.add(rating)
                PlaylistData.comments.add(comment)

                Toast.makeText(this, "Song added to playlist!", Toast.LENGTH_SHORT).show()

                // Clear fields
                edtSongTitle.text.clear()
                edtArtistName.text.clear()
                edtRating.text.clear()
                edtComments.text.clear()
            }

            // View playlist
            btnView.setOnClickListener {
                val intent = Intent(this, DetailActivity::class.java)
                startActivity(intent)
            }

            // Exit app
            btnExit.setOnClickListener {
                finishAffinity()
            }
        }
    }
}
